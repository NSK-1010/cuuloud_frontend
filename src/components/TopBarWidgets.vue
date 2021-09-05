<template>
  <div>
    <v-btn text v-if="!isLogin" @click="$emit('login')">
      <v-badge :value="login" color="deep-purple accent-4"
      content="ログイン" transition="slide-x-transition" >
        <v-hover v-model="login">
          <v-icon>mdi-login</v-icon>
        </v-hover>
      </v-badge>
    </v-btn>
    <v-btn text v-if="!isLogin" @click="$emit('register')">
      <v-badge :value="register" color="deep-purple accent-4"
      content="登録" transition="slide-x-transition" >
        <v-hover v-model="register">
          <v-icon>mdi-account-plus</v-icon>
        </v-hover>
      </v-badge>
    </v-btn>
    <v-btn icon v-if="isLogin" @click="$emit('invite')">
      <v-badge :value="invite" color="deep-purple accent-4"
      content="招待" transition="slide-x-transition" >
        <v-hover v-model="invite">
          <v-icon>mdi-account-multiple-plus</v-icon>
        </v-hover>
      </v-badge>
    </v-btn>
    <v-btn text v-if="isLogin" @click="$emit('logout')">
      <v-badge :value="logout" color="deep-purple accent-4"
      content="ログアウト" transition="slide-x-transition" >
        <v-hover v-model="logout">
          <v-icon>mdi-logout</v-icon>
        </v-hover>
      </v-badge>
    </v-btn>
    <v-avatar v-if="isLogin" color="secondary" size="48">
      <span class="white--text text-h5">{{ name.substr(0,2) }}</span>
    </v-avatar>
  </div>
</template>
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
