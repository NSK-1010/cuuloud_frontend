<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Culoud Logo"
          class="shrink mr-2"
          contain
          :src="require('@/assets/logo.png')"
          transition="scale-transition"
          width="40"
        />
      </div>
      <v-btn text v-if="!isLogin" @click="login">
        <v-icon>mdi-login</v-icon>
      </v-btn>
      <v-btn text v-if="!isLogin" @click="register">
        <v-icon>mdi-account-plus</v-icon>
      </v-btn>
      <v-btn icon v-if="isLogin" @click="invite">
        <v-icon>mdi-account-multiple-plus</v-icon>
      </v-btn>
      <v-btn text v-if="isLogin" @click="logout">
        <v-icon>mdi-logout</v-icon>
      </v-btn>
      <v-avatar v-if="isLogin" style="margin-left: auto;" color="secondary" size="48">
        <span class="white--text text-h5">{{ userName.substr(0,2) }}</span>
      </v-avatar>
      <template v-slot:extension v-if="isLogin">
        <v-tabs v-model="tabModel">
          <v-tab href="#home">Home</v-tab>
          <v-tab v-for="room in joinnedRooms"
          :key="room.id" :href="'#'+room.id">
            {{ room.name }}
          </v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <v-main>
      <v-tabs-items v-model="tabModel" v-if="isLogin">
        <v-tab-item value="home">
          <HomeObject ref="homeObj" @join="join" @create="submitCreateRoom"/>
        </v-tab-item>
        <v-tab-item v-for="room in joinnedRooms"
        :key="room.id" :value="room.id">
          <ChatObject @send="sendMessage" @leave="leave" :ref="room.id"
          :name="room.name" :roomId="room.id">
        </v-tab-item>
      </v-tabs-items>
      <LoginDialog ref="login" @done="submitLogin" />
      <RegisterDialog ref="register" @done="submitRegister" />
      <CreateRoomDialog ref="createRoom" @done="submitCreateRoom" />
      <InviteDialog ref="invite" @done="submitInvite"/>
      <NoticeDialog ref="notice" />
    </v-main>
  </v-app>
</template>
<style>
.scrollbar::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}

.scrollbar::-webkit-scrollbar-track {
  border-radius: 5px;
}

.scrollbar::-webkit-scrollbar-thumb {
  border-radius: 5px;
  background: var(--v-primary-base);
}
</style>
<script>
import io from 'socket.io-client';
import HomeObject from './components/HomeObject.vue';
import CreateRoomDialog from './components/CreateRoomDialog.vue';
import RegisterDialog from './components/RegisterDialog.vue';
import LoginDialog from './components/LoginDialog.vue';
import InviteDialog from './components/InviteDialog.vue';
import NoticeDialog from './components/NoticeDialog.vue';
import ChatObject from './components/ChatObject.vue';

export default {
  name: 'App',

  components: {
    HomeObject,
    ChatObject,
    LoginDialog,
    RegisterDialog,
    CreateRoomDialog,
    InviteDialog,
    NoticeDialog,
  },
  mounted() {
    this.roomSock.on('rooms', (data) => {
      if (this.$refs.homeObj) this.$refs.homeObj.$emit('update', data);
    });
    this.roomSock.on('joinned_rooms', (data) => {
      this.joinnedRooms = data;
    });
    this.roomSock.on('join', (data) => {
      if (data.user_id === this.id) {
        return;
      }
      const obj = data;
      obj.type = 'join';
      this.refs[data.room_id][0].$emit('recieve', obj);
    });
    this.roomSock.on('leave', (data) => {
      const obj = data;
      obj.type = 'leave';
      this.refs[data.room_id][0].$emit('recieve', obj);
    });
    this.roomSock.on('message', (data) => {
      const obj = data;
      obj.type = 'message';
      this.$refs[data.room_id][0].$emit('recieve', obj);
    });
    this.authSock.on('notice', (data) => {
      this.$refs.notice.$emit('open', data.message);
    });
    this.authSock.on('login_error', (data) => {
      this.$refs.notice.$emit('open', data.message);
      this.$refs.login.$emit('stop');
      this.$refs.register.$emit('stop');
    });
    this.authSock.on('login', (data) => {
      if (this.isAuthListening) {
        this.isAuthListening = false;
        this.id = data.id;
        this.userName = data.name;
        this.isLogin = data.login;
        this.$refs.login.$emit('close');
        this.$refs.register.$emit('close');
        if (data.login) {
          this.roomSock.emit('get_all_rooms');
        }
      }
    });
  },
  methods: {
    login() {
      this.$refs.login.$emit('open');
    },
    submitLogin(id, password) {
      this.isAuthListening = true;
      this.authSock.emit('login', { id, password });
      setTimeout(() => {
        if (this.isAuthListening) {
          this.isAuthListening = false;
          this.$refs.login.$emit('stop');
        }
      }, 10000);
    },
    register() {
      this.$refs.register.$emit('open');
    },
    submitRegister(name, id, password, email) {
      this.isAuthListening = true;
      this.authSock.emit('register', {
        name, id, password, email,
      });
      setTimeout(() => {
        if (this.isAuthListening) {
          this.isAuthListening = false;
          this.$refs.register.$emit('stop');
        }
      }, 10000);
    },
    logout() {
      this.isAuthListening = true;
      this.authSock.emit('logout');
      setTimeout(() => {
        if (this.isAuthListening) {
          this.isAuthListening = false;
        }
      }, 10000);
    },
    createRoom() {
      this.$refs.createRoom.$emit('open');
    },
    submitCreateRoom(name) {
      this.roomSock.emit('create_room', { name });
    },
    join(room) {
      this.roomSock.emit('join_room', { room });
    },
    error() {

    },
    leave(roomId) {
      this.roomSock.emit('leave_room', { room: roomId });
    },
    send() {

    },
    invite() {
      this.$refs.invite.$emit('open');
    },
    submitInvite(email) {
      this.authSock.emit('invite', { email });
    },
    sendMessage(roomId, text) {
      this.isListening = true;
      this.roomSock.emit('message', { text, id: roomId });
    },
  },
  data() {
    return {
      id: '',
      domain: '',
      authSock: io.connect('localhost:5000/auth'),
      roomSock: io.connect('localhost:5000/room'),
      isLogin: false,
      joinnedRooms: [],
      userName: '',
      tabModel: 'home',
      debug: 'none',
      isAuthListening: true,
    };
  },
};
</script>
