<template>
<div>
  <div
  style="overflow:auto;
         max-height:100%;">
    <v-card
    style="margin:10px;"
    :class="{
      'center': chat.type !== 'message',
      'left': chat.user_id === userId && chat.type === 'message',
      'right': chat.user_id !== userId && chat.type === 'message',
    }"
    v-for="chat in chatlog" :key="chat.created_at">
      <v-card-text v-if="chat.type === 'message'"> {{ chat.text }} </v-card-text>
      <v-card-text v-if="chat.type === 'join'"> {{ chat.user_name }} が参加しました</v-card-text>
      <v-card-text v-if="chat.type === 'leave'"> {{ chat.user_name }} が退出しました</v-card-text>
      <v-card-text style="font-size: 0.3em">{{ chat.created_at }}</v-card-text>
    </v-card>
  </div>
  <div style="padding:1%;">
    <v-card style="max-width:100%">
      <v-text-field v-model="text" @keyup.enter="send"/>
      <v-icon @click="send">mdi-send</v-icon>
    </v-card>
  </div>
</div>
</template>
<style scoped>
.balloon-l {
  clear:  both;
  float: left;
  width: 30%;
}

.right {
  float: right;
  clear:  both;
  width: 30%;
}

</style>
<script>
export default {
  name: 'ChatObject',
  props: ['name', 'roomId', 'userId'],
  created() {
    this.$on('recieve', (data) => {
      console.log(data);
      this.chatlog.push(data);
    });
  },
  methods: {
    send() {
      this.$emit('send', this.roomId, this.text);
      this.text = '';
    },
  },
  data() {
    return {
      chatlog: [],
      text: '',
      messages: {},
    };
  },
};
</script>
