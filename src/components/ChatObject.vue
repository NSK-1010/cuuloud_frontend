<template>
<div>
  <div v-for="chat in chatlog" :key="chat.created_at">
    {{ chat.created_at}}
    <div
    class="{
      no-balloon: chat.type !== 'message',
      balloon-l: chat.user_id === myId && chat.type === 'message',
      balloon-r: chat.user_id !== myId && chat.type === 'message',
      }">
      <p v-if="chat.type === 'message'"> {{ chat.text }} </p>
      <p v-if="chat.type === 'join'"> {{ chat.user_name }} が参加しました</p>
      <p v-if="chat.type === 'leave'"> {{ chat.user_name }} が退出しました</p>
    </div>
  </div>
  <div style="padding: 1%;position: fixed;bottom: 0px;width:100%;">
    <v-card>
      <v-card-text>
        <div style="display: flex;">
          <v-text-field v-model="text" @keyup.enter="send"/>
          <v-btn @click="send">送信</v-btn>
        </div>
      </v-card-text>
    </v-card>
  </div>
</div>
</template>
<style scoped>
.balloon-l {
  position: relative;
  display: inline-block;
  margin: 1.5em 0 1.5em 15px;
  padding: 7px 10px;
  min-width: 120px;
  max-width: 100%;
  color: #555;
  font-size: 16px;
  background: #FFF;
  border: solid 3px #555;
  box-sizing: border-box;
}

.balloon-l:before {
  content: "";
  position: absolute;
  top: 50%;
  left: -24px;
  margin-top: -12px;
  border: 12px solid transparent;
  border-right: 12px solid #FFF;
  z-index: 2;
}

.balloon-l:after {
  content: "";
  position: absolute;
  top: 50%;
  left: -30px;
  margin-top: -14px;
  border: 14px solid transparent;
  border-right: 14px solid #555;
  z-index: 1;
}

.balloon-l p {
  margin: 0;
  padding: 0;
}

.balloon-r {
  position: relative;
  display: inline-block;
  margin: 1.5em 15px 1.5em 0;
  padding: 7px 10px;
  min-width: 120px;
  max-width: 100%;
  color: #555;
  font-size: 16px;
  background: #FFF;
  border: solid 3px #555;
  box-sizing: border-box;
}

.balloon-r:before {
  content: "";
  position: absolute;
  top: 50%;
  right: -24px;
  margin-top: -12px;
  border: 12px solid transparent;
  border-left: 12px solid #FFF;
  z-index: 2;
}

.balloon-r:after {
  content: "";
  position: absolute;
  top: 50%;
  right: -30px;
  margin-top: -14px;
  border: 14px solid transparent;
  border-left: 14px solid #555;
  z-index: 1;
}

.balloon-r p {
  margin: 0;
  padding: 0;
}

.no-balloon {
  position: relative;
  display: inline-block;
  margin: 1.5em 0 1.5em 0;
  padding: 7px 10px;
  min-width: 120px;
  max-width: 100%;
  color: #555;
  font-size: 16px;
  background: #FFF;
  border: solid 3px #555;
  box-sizing: border-box;
}

.no-balloon p {
  margin: 0;
  padding: 0;
}

</style>
<script>
export default {
  name: 'ChatObject',
  props: ['name', 'roomId', 'myId'],
  created() {
    this.$on('recieve', (data) => {
      this.chatlog.push(data);
    });
  },
  methods: {
    send() {
      this.$emit('send', this.id, this.text);
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
