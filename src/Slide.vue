<template>
  <div class="container">
    <div class="slider">
      <div class="card"
        v-for="(card, idx) in cardList"
        v-if="idx < 2"
        :class="{
          'front': idx === 0,
          'back': idx === 1,
          'slideleft': idx === 0 && slideLeft,
          'slideright': idx === 0 && slideRight,
          'popup': idx === 1 && popup
        }"
        @touchstart="startHandler"
        @touchmove="moveHandler"
        @touchend="releaseHandler"
        @transitionend="transitionendHandler"
        >
        <div class="header">
          <strong>{{ card.title }}</strong>
        </div>
        <div class="content">
          <img :src="card.img">
        </div>
      </div>
    </div>
    <div class="toolbar">
      <div class="resolve">
        <button @click="resolve">Resolve</button>
      </div>
      <div class="reject">
        <button @click="reject">Reject</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      cardList: [
        {
          title: "Happy New Year",
          img: 'http://7xpo7m.com1.z0.glb.clouddn.com/avatar.jpg'
        },
        {
          title: "Happy 2017",
          img: 'http://7xpo7m.com1.z0.glb.clouddn.com/%E9%95%BF%E8%8D%893.jpg'
        }
      ],
      animating: false,
      slideLeft: false,
      slideRight: false,
      popup: false,
      textCounter: 0,
      moveStart: 0,
      moveDistance: 0
    }
  },
  methods: {
    clickHandler () {
      if (this.animating) return
      if (Math.random() >= 0.5) {
        this.slideRight = true
      } else {
        this.slideLeft = true
      }
      this.popup = true
      this.animating = true
    },
    transitionendHandler () {
      this.cardList.splice(0,1)
      this.slideLeft = false
      this.slideRight = false
      this.popup = false
      this.cardList.push({
        title: "Test" + this.textCounter++,
        img: Math.random() >= 0.5 ? 'http://7xpo7m.com1.z0.glb.clouddn.com/avatar.jpg' : 'http://7xpo7m.com1.z0.glb.clouddn.com/%E9%95%BF%E8%8D%893.jpg'
      })
      this.animating = false
    },
    startHandler (e) {
      this.moveStart = e.targetTouches[0].clientX
    },
    moveHandler (e) {
      if (!this.moveStart) return
      let moveDistance = e.targetTouches[0].clientX - this.moveStart
      if (moveDistance > 64) {
        this.reject()
        this.moveStart = 0
      } else if (moveDistance < -64) {
        this.resolve()
        this.moveStart = 0
      }
    },
    releaseHandler (e) {
      this.moveStart = 0
    },
    resolve () {
      if (this.animating) return
      this.slideLeft = true
      this.popup = true
      this.animating = true
    },
    reject () {
      if (this.animating) return
      this.slideRight = true
      this.popup = true
      this.animating = true
    }
  }
}
</script>

<style lang="stylus">
*
  box-sizing border-box
body
  margin 0
  padding 0
  overflow hidden
  background-color #C6F1E7
  font-family "Lucida Grande","Lucida Sans Unicode","Lucida Sans",Geneva,Arial,sans-serif
.container
  position absolute
  top 0
  left 50%
  bottom 0
  width 100%
  max-width 768px
  transform translateX(-50%)
.slider
  position relative
  padding 20px
  width 100%
  margin-top 20px
.card
  position absolute
  width calc(100% - 40px)
  border-radius 5px
  overflow hidden
  background-color #F0FFF3
  &.front
    z-index 10
  &.back
    box-shadow 0 5px 30px rgba(0,0,0,.2)
  .header
    padding 20px 20px
    font-weight bolder
    color #2C3D4F
  .content
    font-size 0
    img
      width 100%

.slideleft
  transform rotate3d(0,0,1,90deg)
  transform-origin -15% -15%
  transition transform .6s ease-out

.slideright
  transform rotate3d(0,0,-1,90deg)
  transform-origin 115% -15%
  transition transform .6s ease-out

.popup
  animation-name popup
  animation-duration .5s
  animation-iteration-count 1

@keyframes popup
  from
    transform scale(.80)
  50%
    transform scale(1.15)
  70%
    transform scale(.95)
  85%
    transform scale(1.02)
  to
    transform scale(1)

@keyframes slideleft
  from
    transform rotate3d(0,0,0,0)
    transform-origin -15% -15%
  to
    transform rotate3d(0,0,1,90deg)
    transform-origin -15% -15%

@keyframes slideright
  from
    transform rotate3d(0,0,0,0)
    transform-origin 115% -15%
  to
    transform rotate3d(0,0,-1,90deg)
    transform-origin 115% -15%

.toolbar
  position absolute
  bottom 10px
</style>
