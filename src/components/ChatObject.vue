<template>
<div>
  <div :id="'msarea-'+roomId"
  class="scrollbar" style="
  overflow: auto;
  height: calc(99vh - 172px);">
    <v-card
    style="margin:10px;"
    outlined
    :class="{
      'center': chat.type !== 'message',
      'left': chat.user_id === userId && chat.type === 'message',
      'right': chat.user_id !== userId && chat.type === 'message',
    }"
    v-for="chat in chatlog" :key="chat.created_at">
      <v-card-text>
        <v-list-item class="grow">
          <v-list-item-content>
            <v-list-item-title>
              {{ showDate(chat.created_at) }}
            </v-list-item-title>
          </v-list-item-content>
          <v-row
          align="center"
          justify="end"
          >
            <span class="subheading" v-if="chat.type === 'message'">
              {{ chat.user_name }}
            </span>
          </v-row>
        </v-list-item>
      </v-card-text>
      <v-card-text v-if="chat.type === 'message'">
        {{ chat.text }}
      </v-card-text>
      <v-card-text class="text-h5" v-if="chat.type === 'join'">
        {{ chat.user_name }} が参加しました
      </v-card-text>
      <v-card-text class="text-h5" v-if="chat.type === 'leave'">
        {{ chat.user_name }} が退出しました
      </v-card-text>
    </v-card>
  </div>
  <v-card tile height="64">
    <v-card-actions>
      <v-text-field height="48" outlined v-model="text" append-outer-icon="mdi-send"
      @keyup.enter="send" @click:append-outer="send" />
    </v-card-actions>
  </v-card>
</div>
</template>
<style scoped>
.left {
  clear:  both;
  float: left;
  width: 40vw;
}

.right {
  float: right;
  clear:  both;
  width: 40vw;
}

.center {
  margin-left: auto;
  margin-right: auto;
  width: 70vw;
}
</style>
<script>
export default {
  name: 'ChatObject',
  props: ['name', 'roomId', 'userId'],
  mounted() {
    this.$on('recieve', (data) => {
      const messagesArea = document.getElementById(`msarea-${this.roomId}`);
      if (messagesArea.scrollHeight - (messagesArea.scrollTop + messagesArea.clientHeight) <= 0) {
        this.chatlog.push(data);
        this.$nextTick(() => {
          messagesArea.scrollTop = messagesArea.scrollHeight;
        });
      } else {
        this.chatlog.push(data);
      }
    });
  },
  methods: {
    send() {
      this.$emit('send', this.roomId, this.text);
      this.text = '';
    },
    showDate(date) {
      const d = new Date(Date.parse(date));
      return `${d.getFullYear()}/${d.getMonth() + 1}/${d.getDate()} ${d.getHours()}:${d.getMinutes()}:${d.getSeconds()} `;
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
