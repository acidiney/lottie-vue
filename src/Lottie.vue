<template>
  <div ref="lottieContainer" :style="style" />
</template>

<script>
import LottieWeb from "lottie-web";
export default {
  name: "Lottie",
  props: {
    /**
     * @description height of animation container, can be String or Number (for pixels)
     */
    height: {
      // type: String || Number,
      default: "100%"
    },
    /**
     * @description width of animation container, can be String or Number (for pixels)
     */
    width: {
      // type: String ||Number,
      default: "100%"
    },
    /**
     * @description Animation props can receive a loaded file or only a string representing a url
     */
    animation: {
      type: Object || String,
      required: true
    },
    /**
     * @description choose the renderer method will be used, by default is svg
     */
    render: {
      type: String,
      default: 'svg',
      validator: (value) => ['svg', 'canvas', 'html'].includes(value)
    },
    /**
     * @description speed of animation
     */
    speed: {
      type: Number,
      default: 1
    },
    /**
     * @description decide if animation will start when animation was loaded
     */
    autoPlay: {
      type: Boolean
    },
    /**
     * @description animation will never stop
     */
    loop: {
      type: Boolean
    },
  },
  methods: {
    /**
     * @description receive name of lottie event and name that the event will be called on vue instace
     */
    dispatchEvent (lottieEvent, emitedName) {
      this.lottie.addEventListener(lottieEvent, (event) => {
        this.$emit(emitedName, event)
      })
    }
  },
  data() {
    return {
      lottie: null,
      style: {
        height: typeof this.height === 'number' ? this.height + 'px' : this.height,
        width: typeof this.width === 'number' ? this.width + 'px' : this.width,
        position: 'relative'
      },
      options: {
        renderer: this.render,
        [typeof this.animation === "object" ? "animationData" : "path"]: this
          .animation,
          autoplay: this.autoPlay,
          loop: this.loop
      },
      lottieEvents: {
        complete: 'onComplete',
        loopComplete: 'onLoopComplete',
        enterFrame: 'onEnterFrame',
        segmentStart: 'onSegmentStart',
        config_ready: 'onConfigReady',
        data_ready: 'onDataReady',
        data_falied: 'onDataFailed',
        loaded_images: 'onLoadedImages',
        DOMLoaded: 'onDOMLoaded',
        destroy: 'onDestroy',
      }
    };
  },
  mounted() {
   
    // create an instance of lottie with default properties
    this.lottie = LottieWeb.loadAnimation({
      container: this.$refs.lottieContainer,
      ...this.options,
    });


    // set speed of animation
    this.lottie.setSpeed(this.speed)

    /**
     * @description will dispatch all events of lottie
     */
    Object.entries(this.lottieEvents).forEach(currentEntry => {
      this.dispatchEvent(currentEntry[0], currentEntry[1])
    })
  }
};
</script>