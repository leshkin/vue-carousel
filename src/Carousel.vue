<template>
  <div ref="carousel" class="carousel" @touchstart="touchStart" @touchend="touchEnd" @touchmove="touchMove">
    <transition-group
      ref="track"
      class='track'
      tag="div"
      :style="`transform: translateX(${offsetX + moveX}px);`">
      <div
        v-for="slide in slides" 
        class='slide'
        :key="slide.id">
        <h4> {{ slide.title }} </h4>
      </div>
    </transition-group>
  </div>
</template>

<script>
export default {
  data () {
    return {
      slides: [
        {
          title: 'I am Slide 1',
          id: 1
        },
        {
          title: 'I am Slide 2',
          id: 2
        },
        {
          title: 'I am Slide 3',
          id: 3
        },
        {
          title: 'I am Slide 4',
          id: 4
        },
        {
          title: 'I am Slide 5',
          id: 5
        }
      ],
      offsetX: 0,
      moveX: 0,
      startX: 0,
    }
  },

  methods: {
    touchStart(event) {
      this.startX = event.touches[0].clientX
    },

    touchEnd(event) {
      if (this.startX - event.changedTouches[0].clientX > 30) {
        this.next()
      }
      if (this.startX - event.changedTouches[0].clientX < -30) {
        this.previous()
      }
      this.moveX = 0
    },

    touchMove(event) {
      this.moveX = event.changedTouches[0].clientX - this.startX
    },

    next () {
      const first = this.slides.shift()
      this.slides = this.slides.concat(first)
    },

    previous () {
      const last = this.slides.pop()
      this.slides = [last].concat(this.slides)
    }
  },

  mounted() {
    const carouselWidth = this.$refs.carousel.getBoundingClientRect().width
    const trackWidth = this.$refs.track.$el.scrollWidth
    this.offsetX = -1 * (trackWidth - carouselWidth) / 2;
  }
}
</script>

<style>
#app {
  max-width: 1280px;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.carousel {
  overflow-x: hidden;
  border: 1px solid green;
  padding-top: 10px;
  padding-bottom: 10px;
  width: 300px;
}

.track {
  display: flex;
  gap: 20px;
} 

.slide {
  flex: none;
  height: 100px;
  width: 150px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 0.1em dashed #000;
  border-radius: 10px;
  transition: transform 0.3s ease-in-out;
  box-sizing: border-box;
}

.slide:first-child {
  opacity: 0;
}

.slide:last-child {
  opacity: 0;
}
</style>
