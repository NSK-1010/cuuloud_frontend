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
      <v-btn text v-if="!isLogin">
        <span class="mr-2" @click="login">ログイン</span>
      </v-btn>
      <v-btn text v-if="!isLogin">
        <span class="mr-2" @click="register">新しく登録</span>
      </v-btn>
      <v-btn text v-if="isLogin">
        <span class="mr-2" @click="createRoom">部屋を作成</span>
      </v-btn>
      <v-btn text v-if="isLogin">
        <span class="mr-2" @click="invite">招待する</span>
      </v-btn>
      <v-btn text v-if="isLogin">
        <span class="mr-2" @click="logout">ログアウト</span>
      </v-btn>
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
          <HomeObject ref="homeObj" @join="join" />
        </v-tab-item>
        <v-tab-item v-for="room in joinnedRooms"
        :key="room.id" :value="room.id">
          <ChatObject @send="sendMessage" :ref="room.id"
          :name="room.name" :roomId="room.id" :userId="id">
        </v-tab-item>
      </v-tabs-items>
      <LoginDialog ref="login" @done="afterLogin" />
      <NoticeDialog ref="notice" />
      <RegisterDialog ref="register" @done="afterRegister" />
      <CreateRoomDialog ref="createRoom" @done="afterCreateRoom" />
    </v-main>
  </v-app>
</template>

<script>
import io from 'socket.io-client';
import HomeObject from './components/HomeObject.vue';
import CreateRoomDialog from './components/CreateRoomDialog.vue';
import RegisterDialog from './components/RegisterDialog.vue';
import LoginDialog from './components/LoginDialog.vue';
import NoticeDialog from './components/NoticeDialog.vue';
import ChatObject from './components/ChatObject.vue';

export default {
  name: 'App',

  components: {
    HomeObject,
    ChatObject,
    NoticeDialog,
    LoginDialog,
    RegisterDialog,
    CreateRoomDialog,
  },
  mounted() {
    this.roomSock.on('rooms', (data) => {
      if (this.$refs.homeObj) this.$refs.homeObj.$emit('update', data);
    });
    this.roomSock.on('joinned_rooms', (data) => {
      this.joinnedRooms = data;
    });
    this.roomSock.on('join', (data) => {
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
    this.authSock.on('error', (data) => {
      this.$refs.notice.emit('open', data.message);
    });
    this.authSock.on('login', (data) => {
      this.id = data.id;
      this.isLogin = data.login;
      if (data.login) {
        this.$refs.login.$emit('close');
        this.roomSock.emit('get_all_rooms');
      }
    });
  },
  methods: {
    login() {
      this.$refs.login.$emit('open');
    },
    afterLogin(id, password) {
      this.authSock.emit('login', { id, password });
      // this.isLogin = true;
    },
    register() {
      this.$refs.register.$emit('open');
    },
    afterRegister(name, id, password, email) {
      this.authSock.emit('register', {
        name, id, password, email,
      });
    },
    logout() {
      this.authSock.emit('logout');
    },
    afterLogout() {
      this.$refs.login.$emit('open');
    },
    createRoom() {
      this.$refs.createRoom.$emit('open');
    },
    afterCreateRoom(name) {
      this.roomSock.emit('create_room', { name });
    },
    join(room) {
      this.roomSock.emit('join_room', { room });
    },
    leave() {

    },
    send() {

    },
    invite() {
    },
    sendMessage(roomId, text) {
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
    };
  },
};
</script>
