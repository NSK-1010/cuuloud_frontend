<template>
  <v-dialog v-model="dialog">
    <v-card>
      <v-card-title>部屋を作成</v-card-title>
      <v-card-text>
        <v-form @submit.prevent>
          <v-text-field label="部屋の名前" v-model="name" />
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-btn class="info" @click="submit">作成</v-btn>
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
      this.$emit('done', this.name);
      this.dialog = false;
    },
  },
  data() {
    return {
      name: '',
      dialog: false,
    };
  },
};
</script>
