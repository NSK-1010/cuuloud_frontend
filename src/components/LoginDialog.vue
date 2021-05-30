<template>
  <v-dialog v-model="dialog" persistent>
    <v-card>
      <v-card-title>ログイン</v-card-title>
      <v-card-text>
      <v-form>
        <v-text-field label="ユーザーID" v-model="id" />
        <v-text-field
        @click:append="showPassword = !showPassword"
        :type="showPassword ? 'text' : 'password'"
        :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
        label="パスワード" v-model="password" />
      </v-form>
      <v-progress-linear v-if="logining" indeterminate />
      </v-card-text>
      <v-card-actions>
        <v-btn v-if="!logining" class="info" @click="submit">ログイン</v-btn>
        <v-btn v-if="!logining" @click="dialog = false">閉じる</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  name: 'LoginDialog',
  created() {
    this.$on('open', () => {
      this.dialog = true;
    });
    this.$on('close', () => {
      this.dialog = false;
    });
  },
  methods: {
    submit() {
      this.logining = true;
      this.$emit('done', this.id, this.password);
    },
  },
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
