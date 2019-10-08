<template>
  <div class="canvas-wrapper">
    <canvas ref="canvas"></canvas>
    <slot></slot>
    <button @click="erase">Erase</button>
  </div>
</template>

<script>
export default {
  name: 'Canvas',
  data() {
    return {
      // By creating the provider in the data property, it becomes reactive,
      // so child components will update when `context` changes.
      provider: {
        // This is the CanvasRenderingContext that children will draw to.
        context: null,
      },
      prevX: 0,
      currX: 0,
      prevY: 0,
      currY: 0,
      flag: false,
      dot_flag: false,
    }
  },

  // Allows any child component to `inject: ['provider']` and have access to it.
  provide() {
    return {
      provider: this.provider,
    }
  },

  methods: {
    init() {
      this.$refs['canvas'].width = this.$refs['canvas'].parentElement.clientWidth
      this.$refs['canvas'].height = this.$refs[
        'canvas'
      ].parentElement.clientHeight

      for (let i = 10; i < this.$refs['canvas'].width; i += 100) {
        this.provider.context.moveTo(0, i)
        this.provider.context.lineTo(this.$refs['canvas'].width, i)
        this.provider.context.strokeStyle = 'blue'
        this.provider.context.stroke()
      }
      for (let i = 10; i < this.$refs['canvas'].height; i += 100) {
        this.provider.context.moveTo(i, 0)
        this.provider.context.lineTo(i, this.$refs['canvas'].width)
        this.provider.context.stroke()
      }

      this.$refs['canvas'].addEventListener(
        'mousemove',
        e => {
          this.findxy('move', e)
        },
        false
      )
      this.$refs['canvas'].addEventListener(
        'mousedown',
        e => {
          this.findxy('down', e)
        },
        false
      )
      this.$refs['canvas'].addEventListener(
        'mouseup',
        e => {
          this.findxy('up', e)
        },
        false
      )
      this.$refs['canvas'].addEventListener(
        'mouseout',
        e => {
          this.findxy('out', e)
        },
        false
      )
    },

    draw(prevX, prevY, currX, currY) {
      this.provider.context.beginPath()
      this.provider.context.moveTo(prevX, prevY)
      this.provider.context.lineTo(currX, currY)
      this.provider.context.strokeStyle = 'black'
      this.provider.context.lineWidth = 5
      this.provider.context.stroke()
      this.provider.context.closePath()
    },

    findxy(res, e) {
      if (res == 'down') {
        this.prevX = this.currX
        this.prevY = this.currY
        this.currX = e.clientX - this.$refs['canvas'].offsetLeft
        this.currY = e.clientY - this.$refs['canvas'].offsetTop

        this.flag = true
        this.dot_flag = true
        if (this.dot_flag) {
          this.provider.context.beginPath()
          this.provider.context.fillStyle = 'black'
          this.provider.context.fillRect(this.currX, this.currY, 2, 2)
          this.provider.context.closePath()
          this.dot_flag = false
        }
      }
      if (res == 'up' || res == 'out') {
        this.flag = false
      }
      if (res == 'move') {
        if (this.flag) {
          this.prevX = this.currX
          this.prevY = this.currY
          this.currX = e.clientX - this.$refs['canvas'].offsetLeft
          this.currY = e.clientY - this.$refs['canvas'].offsetTop
          this.draw(this.prevX, this.prevY, this.currX, this.currY)
        }
      }
    },

    erase() {
      this.init()
    },
  },

  mounted() {
    // We can't access the rendering context until the canvas is mounted to the DOM.
    // Once we have it, provide it to all child components.
    this.provider.context = this.$refs['canvas'].getContext('2d')

    // Resize the canvas to fit its parent's width.
    // Normally you'd use a more flexible resize system.
    this.init()
  },
}
</script>

<style scoped lang="scss">
.canvas-wrapper {
  width: 520px;
  height: 520px;
  margin: auto;
  cursor: crosshair;
}
</style>
