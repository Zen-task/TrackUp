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
      studyDuration: 25, 
      breakDuration: 5, 
      studyTimeRemaining: 25 * 60, 
      breakTimeRemaining: 5 * 60, 
      intervalId: null,
      isStudy: true, 
    };
  },
  computed: {
    studyMinutes() {
      return Math.floor(this.studyTimeRemaining / 60);
    },
    studySecondsFormatted() {
      const seconds = this.studyTimeRemaining % 60;
      return seconds < 10 ? "0" + seconds : seconds;
    },
    breakMinutes() {
      return Math.floor(this.breakTimeRemaining / 60);
    },
    breakSecondsFormatted() {
      const seconds = this.breakTimeRemaining % 60;
      return seconds < 10 ? "0" + seconds : seconds;
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
          this.switchToBreak();
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
          this.switchToStudy();
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
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  position: absolute;
  top: 0;
  left: 0;
  padding: 20px;
}

.timer-block {
  text-align: left;
  padding: 10px;
  margin: 10px 0;
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
