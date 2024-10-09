<template>
  <div class="centered-container">
    <button @click="toggleShow" class="todo_input">button</button>
    <transition name="bounce">
      <p v-if="show">Vue.jsでリッチなインタラクション!</p>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      show: false
    }
  },
  methods: {
    toggleShow() {
      this.show = !this.show;
    }
  }
}
</script>

<style scoped>
.centered-container {
  display: flex;
  justify-content: center;  /* 水平方向の中央揃え */
  align-items: center;      /* 垂直方向の中央揃え */
  min-height: 100vh;        /* ビューポート全体の高さに設定 */
}

</style>
