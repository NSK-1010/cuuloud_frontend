<template>
  <v-dialog v-model="dialog" persistent>
    <v-card>
      <v-card-title>参加する（招待されたメールアドレスのみ）</v-card-title>
      <v-card-text>
        <v-form>
          <v-text-field label="ニックネーム"
          counter="10" v-model="name" type="text" autocomplete="nickname" />
          <v-text-field label="ユーザーID"
          counter="15" v-model="id" type="text" autocomplete="username" />
          <v-text-field label="メールアドレス" type="email" autocomplete="email" v-model="email" />
          <v-text-field
          @click:append="showPassword = !showPassword"
          :type="showPassword ? 'text' : 'password'"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          label="パスワード" v-model="password" @keyup.enter="submit"/>
        </v-form>
        <v-progress-linear v-if="registing" indeterminate />
      </v-card-text>
      <v-card-actions v-if="!registing">
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
      this.registing = false;
      this.dialog = true;
    });
    this.$on('stop', () => {
      this.registing = false;
    });
    this.$on('close', () => {
      this.registing = false;
      this.dialog = false;
    });
  },
  methods: {
    submit() {
      this.registing = true;
      this.$emit('done', this.name, this.id, this.password, this.email);
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
      registing: false,
    };
  },
};
</script>
