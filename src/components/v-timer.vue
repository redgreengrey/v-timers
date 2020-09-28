<template>
  <div class="v-timers" :class="{ 'active-timer': isRunning }">
    <div class="container">
      <div class="container__time">
        <div class="hours" v-if="hours >= 1">{{ hours }} :</div>
        <div v-if="minutes >= 1">
          <div class="minutes" v-if="minutes <= 9 && hours < 1">
            {{ minutes }} :
          </div>
          <div class="minutes" v-if="minutes <= 9 && hours >= 1">
            {{ minutes | zeroPad }} :
          </div>
          <div class="minutes" v-if="minutes > 9">
            {{ minutes | zeroPad }} :
          </div>
        </div>
        <div class="seconds" v-if="seconds >= 0 && minutes < 1">
          {{ seconds }}
        </div>
        <div class="seconds" v-if="minutes > 0">
          {{ seconds | zeroPad }}
        </div>
      </div>
      <div class="container__buttons">
        <div class="buttons">
          <div class="buttons__item" @click="startTimer" v-if="!isRunning">
            <div class="play-button"></div>
          </div>
          <div class="buttons__item" @click="stopTimer" v-if="isRunning">
            <div class="pause-button">
              <div class="pause-button__inner"></div>
            </div>
          </div>
          <div class="buttons__item" @click="clearAll">
            <div
              class="clear-button"
              :class="{ 'clear-button-active': isRunning }"
            ></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      times: [],
      animateFrame: 0,
      nowTime: 0,
      diffTime: 0,
      startTime: 0,
      isRunning: false,
      count: 0
    };
  },
  methods: {
    setSubtractStartTime: function(time) {
      let newTime = typeof time !== "undefined" ? time : 0;
      this.startTime = Math.floor(performance.now() - newTime);
    },
    startTimer: function() {
      var vm = this;
      vm.setSubtractStartTime(vm.diffTime);
      (function loop() {
        vm.nowTime = Math.floor(performance.now());
        vm.diffTime = vm.nowTime - vm.startTime;
        vm.animateFrame = requestAnimationFrame(loop);
      })();
      vm.isRunning = true;
    },
    stopTimer: function() {
      this.isRunning = false;
      cancelAnimationFrame(this.animateFrame);
    },
    pushTime: function() {
      this.times.push({
        hours: this.hours,
        minutes: this.minutes,
        seconds: this.seconds,
        milliSeconds: this.milliSeconds
      });
    },
    clearAll: function() {
      this.startTime = 0;
      this.nowTime = 0;
      this.diffTime = 0;
      this.times = [];
      this.stopTimer();
      this.animateFrame = 0;
    }
  },
  computed: {
    hours: function() {
      return Math.floor(this.diffTime / 1000 / 60 / 60);
    },
    minutes: function() {
      return Math.floor(this.diffTime / 1000 / 60) % 60;
    },
    seconds: function() {
      return Math.floor(this.diffTime / 1000) % 60;
    }
  },
  filters: {
    zeroPad: function(value, num) {
      let rounded = typeof num !== "undefined" ? num : 2;
      return value.toString().padStart(rounded, "0");
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  width: 225px;
  height: 120px;
  @include flexible();
  flex-direction: column;
  background-color: $color-block;

  &__time {
    @include flexible();
    width: 100%;
    height: 50%;
    border-bottom: 1px solid $text-color;
  }

  &__buttons {
    @include flexible();
    width: 100%;
    height: 50%;
  }
}

@media (max-width: 767px) {
  .container {
    margin-bottom: 45px;
  }
}

.buttons {
  width: 85px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: space-around;

  &__item {
    cursor: pointer;
    letter-spacing: 1px;

    &-pause {
      background-color: white;
      display: inline-block;
    }

    &-inner {
      background-color: black;
      border-width: 0px 0px 0px 10px;
      border-color: white;
      border-style: double;
      width: 5px;
      height: 20px;
      margin-left: 5px;
    }
  }
}

.seconds,
.minutes {
  min-width: 25px;
}

.minutes {
  padding-left: 3px;
}

.active-timer {
  color: #fff;
  border-color: #fff;
}

.play-button {
  border: 10px solid transparent;
  border-bottom: 16px solid;
  margin: 0px 0 0 10px;
  transform: rotate(90deg);
}

.clear-button {
  width: 20px;
  height: 20px;
  background-color: $text-color;

  &-active {
    background-color: #fff;
  }
}

.pause-button {
  background-color: transparent;
  display: inline-block;
}

.pause-button__inner {
  background-color: $color-block;
  border-width: 0px 0px 0px 10px;
  border-color: white;
  border-style: double;
  width: 5px;
  height: 20px;
  margin-left: 5px;
}
</style>
