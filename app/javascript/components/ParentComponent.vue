<template>
  <div class="timer-container">
    <div class="timer-block">
      <h1>学習タイマー</h1>
      <div class="timer-display">
        <span>{{ studyMinutes }}:{{ studySecondsFormatted }}</span>
      </div>
      <input type="number" v-model="studyDuration" placeholder="分を設定">
      <button @click="startStudyTimer">スタート</button>
      <button @click="resetStudyTimer">リセット</button>
    </div>

    <div class="timer-block">
      <h1>休憩タイマー</h1>
      <div class="timer-display">
        <span>{{ breakMinutes }}:{{ breakSecondsFormatted }}</span>
      </div>
      <input type="number" v-model="breakDuration" placeholder="分を設定">
      <button @click="startBreakTimer">スタート</button>
      <button @click="resetBreakTimer">リセット</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      studyDuration: 25, // ユーザーが設定する学習時間（分）
      breakDuration: 5, // ユーザーが設定する休憩時間（分）
      studyTimeRemaining: 25 * 60, // 学習タイマーの残り時間（秒）
      breakTimeRemaining: 5 * 60, // 休憩タイマーの残り時間（秒）
      intervalId: null,
      isStudy: true, // 現在のタイマーが学習中かどうか
    };
  },
  computed: {
    studyMinutes() {
      return Math.floor(this.studyTimeRemaining / 60);
    },
    studySeconds() {
      return this.studyTimeRemaining % 60;
    },
    studySecondsFormatted() {
      return this.studySeconds < 10 ? "0" + this.studySeconds : this.studySeconds;
    },
    breakMinutes() {
      return Math.floor(this.breakTimeRemaining / 60);
    },
    breakSeconds() {
      return this.breakTimeRemaining % 60;
    },
    breakSecondsFormatted() {
      return this.breakSeconds < 10 ? "0" + this.breakSeconds : this.breakSeconds;
    },
  },
  methods: {
    startStudyTimer() {
      this.isStudy = true;
      if (this.intervalId) clearInterval(this.intervalId);
      this.intervalId = setInterval(() => {
        if (this.studyTimeRemaining > 0) {
          this.studyTimeRemaining--;
        } else {
          this.switchToBreak(); // 学習タイマー終了後、休憩タイマーを開始
        }
      }, 1000);
    },
    resetStudyTimer() {
      clearInterval(this.intervalId);
      this.studyTimeRemaining = this.studyDuration * 60;
    },
    startBreakTimer() {
      this.isStudy = false;
      if (this.intervalId) clearInterval(this.intervalId);
      this.intervalId = setInterval(() => {
        if (this.breakTimeRemaining > 0) {
          this.breakTimeRemaining--;
        } else {
          this.switchToStudy(); // 休憩タイマー終了後、学習タイマーを開始
        }
      }, 1000);
    },
    resetBreakTimer() {
      clearInterval(this.intervalId);
      this.breakTimeRemaining = this.breakDuration * 60;
    },
    switchToBreak() {
      clearInterval(this.intervalId);
      this.breakTimeRemaining = this.breakDuration * 60;
      this.startBreakTimer();
    },
    switchToStudy() {
      clearInterval(this.intervalId);
      this.studyTimeRemaining = this.studyDuration * 60;
      this.startStudyTimer();
    },
  },
};
</script>

<style scoped>
.timer-container {
  display: flex;
  flex-direction: column;  /* タイマーを縦に並べる */
  align-items: flex-start; /* 左寄せにする */
  justify-content: flex-start; /* 上寄せにする */
  position: absolute; /* 固定位置 */
  top: 0; /* 上端に固定 */
  left: 0; /* 左端に固定 */
  padding: 20px;
}

.timer-block {
  text-align: left;
  padding: 10px;
  margin: 10px 0;  /* タイマー間のスペースを確保 */
  border: 1px solid #ccc;
  border-radius: 5px;
}

.timer-display {
  font-size: 2.5em;
  margin: 15px 0;
}

button {
  padding: 10px;
  margin: 5px;
  font-size: 1.2em;
}
</style>

