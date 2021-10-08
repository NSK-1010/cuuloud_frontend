<template>
  <v-dialog v-model="dialog" :persistent="logining">
    <v-card>
      <v-card-title>ログイン</v-card-title>
      <v-card-text>
      <v-form>
        <v-text-field :disabled="logining || disconnected"
        label="ユーザーID" v-model="id" type="text" autocomplete="username" />
        <v-text-field
        @click:append="showPassword = !showPassword"
        :disabled="logining || disconnected"
        :type="showPassword ? 'text' : 'password'"
        :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
        label="パスワード" v-model="password"  @keyup.enter="submit"/>
      </v-form>
      </v-card-text>
      <v-card-actions>
        <v-btn :disabled="disconnected" :loading="logining"
        class="info" type="submit" @click="submit">ログイン</v-btn>
        <v-btn :disabled="logining" @click="dialog = false">閉じる</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  name: 'LoginDialog',
  created() {
    this.$on('open', () => {
      this.logining = false;
      this.dialog = true;
    });
    this.$on('stop', () => {
      this.logining = false;
    });
    this.$on('close', () => {
      this.logining = false;
      this.dialog = false;
    });
  },
  methods: {
    submit() {
      this.logining = true;
      this.$emit('done', this.id, this.password);
    },
  },
  props: ['disconnected'],
  data() {
    return {
      dialog: false,
      showPassword: false,
      id: '',
      password: '',
      logining: false,
    };
  },
};
</script>
