<template>
  <div>
    <v-hover v-model="login">
      <v-btn icon v-if="!isLogin" @click="$emit('login')">
        <v-badge class="hover-badge" :value="login" color="accent"
        content="ログイン" transition="slide-x-transition" >
            <v-icon>mdi-login</v-icon>
        </v-badge>
      </v-btn>
    </v-hover>
    <v-hover v-model="register">
      <v-btn icon v-if="!isLogin" @click="$emit('register')">
        <v-badge class="hover-badge" :value="register" color="accent"
        content="登録" transition="slide-x-transition" >
          <v-icon>mdi-account-plus</v-icon>
        </v-badge>
      </v-btn>
    </v-hover>
    <v-hover v-model="invite">
      <v-btn icon v-if="isLogin" @click="$emit('invite')">
        <v-badge class="hover-badge" :value="invite" color="accent"
        content="招待" transition="slide-x-transition" >
          <v-icon>mdi-account-multiple-plus</v-icon>
        </v-badge>
      </v-btn>
    </v-hover>
    <v-hover v-model="logout">
      <v-btn icon v-if="isLogin" @click="$emit('logout')">
        <v-badge class="hover-badge" :value="logout" color="accent"
        content="ログアウト" transition="slide-x-transition" >
          <v-icon>mdi-logout</v-icon>
        </v-badge>
      </v-btn>
    </v-hover>
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
  props: ['name', 'isLogin'],
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
