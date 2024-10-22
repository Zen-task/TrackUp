<template>
  <div>
    <h1>{{ timerType }}タイマー</h1>
    <div class="timer-display">
      <span>{{ minutes }}:{{ secondsFormatted }}</span>
    </div>
    <button @click="startTimer">スタート</button>
    <button @click="resetTimer">リセット</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeRemaining: 25 * 60, // 作業タイマーの初期設定（秒）
      workDuration: 25 * 60,  // 25分
      breakDuration: 5 * 60,  // 5分
      intervalId: null,
      timerType: '作業', // '作業'または'休憩'
    };
  },
  computed: {
    minutes() {
      return Math.floor(this.timeRemaining / 60);
    },
    seconds() {
      return this.timeRemaining % 60;
    },
    secondsFormatted() {
      return this.seconds < 10 ? "0" + this.seconds : this.seconds;
    },
  },
  methods: {
    startTimer() {
      if (this.intervalId) return; // 既にカウントダウンが進んでいる場合はスキップ
      this.intervalId = setInterval(() => {
        if (this.timeRemaining > 0) {
          this.timeRemaining--;
        } else {
          this.switchTimer(); // タイマー終了後にタイプを切り替える
        }
      }, 1000); // 1秒ごとにカウントダウン
    },
    resetTimer() {
      clearInterval(this.intervalId);
      this.intervalId = null;
      this.timeRemaining = this.timerType === '作業' ? this.workDuration : this.breakDuration; // 現在のタイマータイプに応じてリセット
    },
    switchTimer() {
      clearInterval(this.intervalId);
      this.intervalId = null;
      if (this.timerType === '作業') {
        this.timerType = '休憩';
        this.timeRemaining = this.breakDuration;
        alert("作業タイマーが終了しました！休憩を開始します。");
      } else {
        this.timerType = '作業';
        this.timeRemaining = this.workDuration;
        alert("休憩が終了しました！作業を再開します。");
      }
      this.startTimer(); // タイマーを再スタート
    },
  },
};
</script>

<style scoped>
.timer-display {
  font-size: 3em;
  margin: 20px 0;
}
button {
  padding: 10px 20px;
  font-size: 1.2em;
  margin: 5px;
}
</style>
