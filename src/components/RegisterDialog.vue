<template>
  <v-dialog v-model="dialog" :persistent="registing">
    <v-card>
      <v-card-title>参加する（招待されたメールアドレスのみ）</v-card-title>
      <v-card-text>
        <v-form>
          <v-text-field  label="メールアドレス" :disabled="registing || disconnected"
          type="email" autocomplete="email" v-model="email" />
          <v-text-field label="ニックネーム" :disabled="registing || disconnected"
          counter="10" v-model="name" type="text" autocomplete="nickname" />
          <v-text-field label="ユーザーID" :disabled="registing || disconnected"
          counter="15" v-model="id" type="text" autocomplete="username" />
          <v-text-field
          @click:append="showPassword = !showPassword"
          :disabled="registing || disconnected"
          :type="showPassword ? 'text' : 'password'"
          :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
          label="パスワード" v-model="password" @keyup.enter="submit"/>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-btn :loading="registing" :disabled="disconnected"
        class="info" @click="submit" type="submit">登録</v-btn>
        <v-btn :disabled="registing" @click="dialog = false">閉じる</v-btn>
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
  props: ['disconnected'],
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
