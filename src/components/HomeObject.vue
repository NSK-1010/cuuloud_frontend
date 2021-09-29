<template>
  <div class="scrollbar" style="
  overflow: auto;
  height: calc(100vh - 112px);">
    <v-card style="margin:10px;" elevation="7" shaped>
      <v-card-title>部屋を作成する</v-card-title>
      <v-card-text>
        <v-form @submit.prevent>
          <v-text-field label="部屋の名前" v-model="roomName" @keyup.enter="createRoom"/>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-btn class="info" @click="createRoom">作成</v-btn>
      </v-card-actions>
    </v-card>
    <v-card style="margin:10px;" elevation="7" shaped v-for="room in rooms" :key="room.created_at">
      <v-card-title>{{ room.name }}</v-card-title>
      <v-card-actions>
        <v-btn icon @click="join(room.id)">
          <v-icon>mdi-login-variant</v-icon>
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>
<script>
export default {
  name: 'HomeObject',
  created() {
    this.$on('update', (data) => {
      this.rooms = data;
    });
  },
  mounted() {
    this.$emit('getRooms');
  },
  methods: {
    join(room) {
      this.$emit('join', room);
    },
    createRoom() {
      this.$emit('create', this.roomName);
      this.roomName = '';
    },
  },
  data() {
    return {
      roomName: '',
      rooms: [],
    };
  },
};
</script>
