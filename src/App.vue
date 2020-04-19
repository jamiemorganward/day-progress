<template>
  <div id="app">
    <progress-chart :progress="daylightProgress" />
  </div>
</template>

<script>
import ProgressChart from './components/ProgressChart.vue'
import SunCalc from 'suncalc'

export default {
  name: 'app',

  data () {
    return {
      currentTime: 0,
      sunTimes: {}
    }
  },

  components: {
    ProgressChart
  },

  computed: {
    sunriseInDecimal () {
      return this.sunTimes.sunrise.getHours() + this.sunTimes.sunrise.getMinutes() / 60
    },

    sunsetInDecimal () {
      return this.sunTimes.sunset.getHours() + this.sunTimes.sunset.getMinutes() / 60
    },

    currentTimeInDecimal () {
      return this.currentTime.getHours() + this.currentTime.getMinutes() / 60 + this.currentTime.getSeconds() / 3600 + this.currentTime.getMilliseconds() / 3600000
    },

    daylightProgress () {
      if (!this.sunTimes) {
        return 0
      }
      return ((this.currentTimeInDecimal - this.sunriseInDecimal) / this.totalSunHours)
    },

    totalSunHours () {
      return this.sunsetInDecimal - this.sunriseInDecimal
    }
  },

  methods: {
    updateCurrentTime () {
      this.currentTime = new Date()
    }
  },

  created () {
    this.interval = setInterval(() => {
      this.updateCurrentTime()
    }, 10)

    this.updateCurrentTime()
    this.sunTimes = SunCalc.getTimes(new Date(), -41.204238, 174.801150)
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  background-color: #091c27;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

body, html {
  margin: 0;
}

html {
    min-height: 100%;
    display: flex;
}

body {
    flex: 1;
}
</style>
