<template>
  <div class="py-auto my-16" style="height:calc(100vh - 64px);">
    <div>
      <p class="font-weight-light text-center text-h2">
        Cuuloudへようこそ。
      </p>
      <v-img
            alt="Culoud Logo"
            class="shrink mx-auto"
            contain
            :src="require('@/assets/logo_full.png')"
            transition="scale-transition"
            width="50vw"
            />
      <p class="my-8 font-weight-regular text-center">
        ここは、今だけの話題でゆったりと話すことができる場所です。<br>
        左上のボタンから、早速始めてみましょう！
      </p>
    </div>
  </div>
</template>
<style scoped>

</style>
<script>
export default {
  name: 'Welcome',
};
</script>
