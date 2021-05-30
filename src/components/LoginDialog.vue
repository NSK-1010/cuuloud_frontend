<template>
  <v-dialog v-model="dialog">
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
      </v-card-text>
      <v-card-actions>
        <v-btn class="info" @click="submit">登録</v-btn>
        <v-btn @click="dialog = false">閉じる</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  name: 'LoginDialog',
  methods: {
    submit() {
      this.$emit('done', this.id, this.password);
    },
  },
  data() {
    return {
      showPassword: false,
      id: '',
      password: '',
    };
  },
};
</script>
