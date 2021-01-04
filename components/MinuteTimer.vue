<template>
  <div>
    <div class="btn-toolbar row">
      <button class="btn col-2 btn-primary" @click="addMinute">
        +1min
      </button>
      <button class="btn col-2 btn-primary" @click="addSecond">
        +1sec
      </button>
      <button v-if="isRunning" key="stop" class="btn col-2 btn-warning" @click="stop">
        Stop!
      </button>
      <button v-else key="start" class="btn col-2 btn-warning" @click="start">
        Start!
      </button>
    </div>
    <div class="row">
      <div class="timer" :class="isOver">
        {{ remainTimeStr }}
      </div>
    </div>
    <div class="btn-toolbar row">
      <button class="btn col-2 btn-primary" @click="subMinute">
        -1min
      </button>
      <button class="btn col-2 btn-primary" @click="subSecond">
        -1sec
      </button>
      <button class="btn col-2 btn-warning" @click="clear">
        clear
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      remainTime: 0,
      Interval: null,
      isOver: ''
    }
  },
  computed: {
    remainTimeStr () {
      return `${('00' + this.remainMinutes).slice(-2)}:${('00' + this.remainSeconds).slice(-2)}`
    },
    remainMinutes () {
      return Math.floor(this.remainTime / 60)
    },
    remainSeconds () {
      return this.remainTime % 60
    },
    isRunning () {
      return this.Interval != null
    }
  },
  methods: {
    addSecond () {
      this.remainTime += 1
      this.remainTime = Math.min(60 * 59 + 59, this.remainTime)
    },
    addMinute () {
      this.remainTime += 60
      this.remainTime = Math.min(60 * 59 + 59, this.remainTime)
    },
    subMinute () {
      this.remainTime -= 60
      this.remainTime = Math.max(0, this.remainTime)
    },
    subSecond () {
      this.remainTime -= 1
      this.remainTime = Math.max(0, this.remainTime)
    },
    start () {
      if (this.remainTime > 0) { this.Interval = setInterval(this.tick, 1000) }
    },
    tick () {
      this.subSecond()
      if (this.remainTime === 0) {
        this.over()
      }
    },
    over () {
      clearInterval(this.Interval)
      this.Interval = null
      this.isOver = 'overed'
    },
    stop () {
      clearInterval(this.Interval)
      this.Interval = null
    },
    clear () {
      clearInterval(this.Interval)
      this.Interval = null
      this.remainTime = 0
      this.isOver = ''
    }
  }
}
</script>

<style>
.row {
  padding: 10px;
}

.timer {
  font: Roboto;
  font-size: 12ex;
}

.overed {
  color: #ff0000;
}
</style>
