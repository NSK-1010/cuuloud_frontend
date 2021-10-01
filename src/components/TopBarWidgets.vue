<template>
  <div class="d-flex">
    <div v-if="!isLogin">
      <v-tooltip bottom>
        <template v-slot:activator="{ on, attrs }">
          <v-btn :disabled="disconnected" v-bind="attrs" v-on="on" icon @click="$emit('login')">
            <v-icon>mdi-login</v-icon>
          </v-btn>
        </template>
        <span>ログイン</span>
      </v-tooltip>
    </div>
    <div v-if="!isLogin">
      <v-tooltip bottom>
        <template v-slot:activator="{ on, attrs }">
          <v-btn :disabled="disconnected" v-bind="attrs" v-on="on" icon @click="$emit('register')">
            <v-icon>mdi-account-plus</v-icon>
          </v-btn>
        </template>
        <span>登録</span>
      </v-tooltip>
    </div>
    <div v-if="isLogin">
      <v-tooltip bottom>
        <template v-slot:activator="{ on, attrs }">
          <v-btn :disabled="disconnected" v-bind="attrs" v-on="on" icon @click="$emit('invite')">
            <v-icon>mdi-account-multiple-plus</v-icon>
          </v-btn>
        </template>
        <span>招待</span>
      </v-tooltip>
    </div>
    <div v-if="isLogin">
      <v-tooltip bottom>
        <template v-slot:activator="{ on, attrs }">
          <v-btn :disabled="disconnected" v-bind="attrs" v-on="on" icon @click="$emit('logout')">
            <v-icon>mdi-logout</v-icon>
          </v-btn>
        </template>
        <span>ログアウト</span>
      </v-tooltip>
    </div>
    <v-avatar v-if="isLogin" color="secondary" size="48">
      <span class="white--text text-h5">{{ name.substr(0,2) }}</span>
    </v-avatar>
  </div>
</template>
<style scoped>
.hover-badge {
  z-index:1;
}
</style>
<script>
export default {
  name: 'TopBarWidgets',
  props: ['name', 'isLogin', 'disconnected'],
  data() {
    return {
      login: false,
      register: false,
      invite: false,
      logout: false,
    };
  },
};
</script>
