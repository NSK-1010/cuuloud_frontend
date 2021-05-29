<template>
<div>
  <h2>ログイン</h2>
  <v-form>
    <v-text-field label="ユーザーID" v-model="id" />
    <v-text-field
    @click:append="showPassword = !showPassword"
    :type="showPassword ? 'text' : 'password'"
    :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
    label="パスワード" v-model="password" />
  </v-form>
  <v-btn class="info" @click="submit">ログイン</v-btn>
</div>
</template>

<script>
export default {
  name: 'LoginObject',
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
