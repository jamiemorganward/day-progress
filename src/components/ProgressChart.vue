<template>
  <div class="progress">
    <div class="graph" ref="graph" />
    <span>{{ Math.round(progress * 10000) / 100 }}%</span>
  </div>
</template>

<script>
import ProgressBar from 'progressbar.js'
export default {
  props: {
    progress: {
      type: Number,
      default: 0,
      animating: false
    }
  },
  watch: {
    progress () {
      if (this.animating) {
        return
      }
      this.animating = true
      this.circle.animate(this.progress, {}, () => {
        this.animating = false
      })
    }
  },
  mounted () {
    this.circle = new ProgressBar.SemiCircle(this.$refs.graph, {
      color: '#FCB03C',
      strokeWidth: 3,
      trailWidth: 1,
      easing: 'easeInOut'
    })
  }
}
</script>

<style lang="scss" scoped>
.graph {
  width: 300px;
  height: 150px;
}
span {
  position: absolute;
  left: 50%;
  transform: translateX(-50%) translateY(-2rem);
  font-size: 2rem;
}
.progress {
  position: relative;
  text-align: center;
}
</style>
