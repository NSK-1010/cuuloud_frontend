<template>
<div>
  <div :id="'msarea-'+roomId"
  class="scrollbar" style="
  overflow: auto;
  height: calc(99vh - 222px);">
    <v-card
    style="margin:10px;"
    outlined
    v-for="chat in chatlog" :key="chat.created_at">
      <v-card-text>
        <v-card-text class="text-h6" v-if="chat.type === 'message'">
          {{ chat.text }}
        </v-card-text>
        <v-card-text class="text-h6" v-if="chat.type === 'join'">
          {{ chat.user_name }} が参加しました
        </v-card-text>
        <v-card-text class="text-h6" v-if="chat.type === 'leave'">
          {{ chat.user_name }} が退出しました
        </v-card-text>
        <v-list-item class="grow" v-if="chat.type === 'message'">
          <v-list-item-avatar color="grey darken-3">
            <v-img
              class="elevation-6"
              alt=""
              src="https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light"
            ></v-img>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title class="text-h7">{{ chat.user_name }}</v-list-item-title>
          </v-list-item-content>
          <v-row align="center" justify="end">
            <span class="subheading mr-2 text-h7">{{ showDate(chat.created_at) }}</span>
          </v-row>
        </v-list-item>
      </v-card-text>
    </v-card>
  </div>
  <v-card tile outlined height="114">
      <v-card tile height="48">
        <v-card-actions>
          <v-btn icon @click="leave">
            <v-icon>mdi-logout-variant</v-icon>
          </v-btn>
          <v-btn icon disabled>
            <v-icon>mdi-image</v-icon>
          </v-btn>
        </v-card-actions>
    </v-card>
    <v-card-actions>
      <v-text-field height="48" outlined v-model="text" append-outer-icon="mdi-send"
      @keyup.enter="send" @click:append-outer="send" />
    </v-card-actions>
  </v-card>
</div>
</template>
<style scoped>
</style>
<script>
export default {
  name: 'ChatObject',
  props: ['name', 'roomId'],
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
    leave() {
      this.$emit('leave', this.roomId);
    },
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
