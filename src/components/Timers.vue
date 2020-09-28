<template>
  <div class="timers-container">
    <v-timer
      v-for="(timer, index) in timers"
      :key="index"
      :item="timer"
      class="timers-container__v-timers"
    />
    <div @click="addTimer" class="timers-container__add-block">
      +
    </div>
  </div>
</template>

<script>
import vTimer from "./v-timer.vue";

export default {
  name: "Timers",
  components: {
    vTimer
  },
  data() {
    return {
      timers: [
        {
          id: 1,
          name: "Timer 1"
        },
        {
          id: 2,
          name: "Timer 2"
        }
      ]
    };
  },
  methods: {
    addTimer() {
      this.timers.push({
        id: Math.random(),
        name: "Timer " + this.id
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.timers-container {
  margin-bottom: 45px;

  &__add-block {
    @include flexible();
    background-color: $color-block;
    width: 225px;
    height: 120px;
    font-size: 32px;
  }
}

@media (max-width: 767px) {
  .timers-container {
    @include flexible();
    flex-direction: column;
    width: 225px;
  }
}

@media (min-width: 768px) and (max-width: 1023px) {
  .timers-container {
    @include flexible();
    justify-content: flex-start;
    width: 500px;
    flex-wrap: wrap;
    gap: 50px;
  }
}

@media (min-width: 1024px) {
  .timers-container {
    @include flexible();
    justify-content: flex-start;
    flex-wrap: wrap;
    padding: 0 80px;
    margin-left: 150px;
    width: 1046px;
  }

  .timers-container__v-timers,
  .timers-container__add-block {
    @include flexible();
    margin-right: 45px;
    margin-bottom: 45px;
  }

  .timers-container__add-block {
    cursor: pointer;
  }
}
</style>
