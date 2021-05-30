<template>
  <div>
    <v-card style="margin:10px;" elevation="7" shaped v-for="room in rooms" :key="room.created_at">
      <v-card-title>{{ room.name }}</v-card-title>
      <v-card-actions>
        <v-btn @click="join(room.id)">参加</v-btn>
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
    this.$emit('get');
  },
  methods: {
    join(room) {
      this.$emit('join', room);
    },
  },
  data() {
    return {
      rooms: [],
    };
  },
};
</script>
