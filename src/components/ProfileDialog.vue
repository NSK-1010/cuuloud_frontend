<template>
  <v-dialog @click:outside="close" @keydown:Escape="close" v-model="dialog" persistent >
    <v-card>
      <v-img
          height="128px"
          src="https://cdn.pixabay.com/photo/2012/04/14/16/37/sky-34536_960_720.png"
        >
        <v-card-title class="white--text">
          <v-avatar color="secondary" size="48">
            <span class="white--text text-h5">{{ name.substr(0,2) }}</span>
          </v-avatar>
          <div>
            <span class="ml-3 mb-0 ">{{ name }}</span>
            <span class="ml-3 caption">@{{ id }}</span>
          </div>
        </v-card-title>
      </v-img>
      <v-card-text>
        参加した部屋
        <RoomObject v-for="room in rooms" :key="room.created_at"
        :room="room" @join="(id) => $emit('join',id)"/>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>
<script>
import RoomObject from './RoomObject.vue';

export default {
  name: 'ProfileDialog',
  components: {
    RoomObject,
  },
  props: ['disconnected', 'profile'],
  methods: {
    close() {
      this.dialog = false;
      this.$emit('close', this.id);
    },
  },
  data() {
    return {
      dialog: true,
      name: this.profile.name,
      id: this.profile.id,
      rooms: this.profile.rooms,
    };
  },
};
</script>
