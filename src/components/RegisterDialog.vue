<template>
  <v-dialog v-model="dialog">
    <v-card>
      <v-card-title>ログイン</v-card-title>
      <v-card-text>
        <v-form>
          <v-text-field label="ニックネーム" v-model="name" type="text" autocomplete="nickname" />
          <v-text-field label="ユーザーID" v-model="id" type="text" autocomplete="username" />
          <v-text-field label="メールアドレス" type="email" autocomplete="email" v-model="email" />
          <v-text-field
          @click:append="showPassword = !showPassword"
          :type="showPassword ? 'text' : 'password'"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          label="パスワード" v-model="password" @keyup.enter="submit"/>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-btn class="info" @click="submit" type="submit">登録</v-btn>
        <v-btn @click="dialog = false">閉じる</v-btn>
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
  },
  methods: {
    submit() {
      this.$emit('done', this.name, this.id, this.password, this.email);
      this.dialog = false;
    },
  },
  data() {
    return {
      dialog: false,
      showPassword: false,
      name: '',
      id: '',
      password: '',
      email: '',
    };
  },
};
</script>
