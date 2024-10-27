<template>
  <div class="timer-container">
    <div class="timer-block study-timer">
      <div class="timer-header">
        <span>記録する作業</span>
      </div>
      <div class="timer-display">
        <span>{{ studyMinutes }}:{{ studySecondsFormatted }}</span>
      </div>
      <div class="timer-controls">
        <button @click="pauseTimer" class="control-button start-button">▶️</button>
        <button @click="resetStudyTimer" class="control-button reset-button">🔄</button>
      </div>
    </div>

    <div class="timer-block break-timer">
      <div class="timer-header">
        <span>休憩</span>

      </div>
      <div class="timer-display">
        <span>{{ breakMinutes }}:{{ breakSecondsFormatted }}</span>
      </div>
      <div class="timer-controls">
        <button @click="startBreakTimer" class="control-button start-button">▶️</button>
        <button @click="resetBreakTimer" class="control-button reset-button">🔄</button>
      </div>
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
      isPaused: false
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
    }
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
    pauseTimer() {
      if (this.isPaused) {
        this.startStudyTimer();
      } else {
        clearInterval(this.intervalId);
      }
      this.isPaused = !this.isPaused;
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
    }
  }
};
</script>

<style scoped>
.timer-container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-color: #2b2b2b;
  padding: 10px;
  border-radius: 10px;
  color: #fff;
  width: 100%;
}

.timer-block {
  padding: 10px;
  background-color: #3d3d3d;
  margin: 10px 0;
  border-radius: 10px;
  position: relative;
}

.timer-header {
  display: flex;
  justify-content: space-between;
  font-size: 1.2em;
  margin-bottom: 10px;
}

.timer-top-right {
  font-size: 0.8em;
  color: #bbb;
}

.timer-display {
  font-size: 2.8em;
  text-align: center;
  margin-bottom: 10px;
}

.timer-controls {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.control-button {
  font-size: 1.5em;
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.pause-button {
  color: #ff4d4d;
}

.reset-button {
  color: #4d94ff;
}

.start-button {
  color: #4caf50;
}

.study-timer {
  border-left: 5px solid #ffeb3b;
}

.break-timer {
  border-left: 5px solid #4caf50;
}
</style>
