<template lang="pug">
  .slider
    .slider__inner(:style="{'opacity': sliderOpacity}")
      .slider__slide(:style="{'background-color': `${currentSliderContent.color}`}")
        .slider__media(:style="{'background-image': `url(${currentSliderContent.image})`}")
          transition(name="bounce")
            .slider__info(v-if="!animate")
              h2 {{ currentSliderContent.text.topText }}
              h1 {{ currentSliderContent.text.middleText }}
              h5 {{ currentSliderContent.text.bottomText }}
              button.btn.btn--primed.btn--primary.btn--uppercase.btn--big {{ currentSliderContent.buttonText }}
      indicators(ref="indicator",
                 :sliderContent="sliderContent",
                 @slideWasChanged="sliderWasChanged($event), animate = true, opacityAnimate = true")
</template>

<script>
  import {sliderContent} from './slider-content.js';
  import Indicators from './Indicators';

  export default {
    name: "slider",
    data() {
      return {
        sliderContent: sliderContent,
        currentSliderContent: null,
        initialSliderPosition: 0,
        animate: false,
        opacityAnimate: false,
        opacity: 1 // start opacity
      }
    },
    computed: {
      sliderOpacity() {
        return this.opacity = this.opacityAnimate ? 0.8 : 1;
      }
    },
    watch: {
      animate() {
        setTimeout(() => this.animate = false, 550);
      },
      opacityAnimate() {
        setTimeout(() => this.opacityAnimate = false, 250);
      }
    },
    methods: {
      sliderWasChanged(event) {
        this.currentSliderContent = sliderContent[event];
      },
      initialSliderChange(number) {
        this.$refs.denis.changeSlide(number);
      }
    },
    created() {
      this.currentSliderContent = sliderContent[this.initialSliderPosition];
    },
    mounted() {
      this.$refs.indicator.changeSlide(this.initialSliderPosition, true);
    },
    components: {
      Indicators
    }
  }
</script>

<style>

  .bounce-enter {
    opacity: 0;
    transform: translateX(70px);
  }

  .bounce-enter-active {
    transition: all 0.3s ease-out;
  }

  .bounce-leave-active {
    transition: all 0.3s ease-in-out;
  }

  .bounce-leave-to {
    opacity: 0;
    transform: translateX(90px);
  }
</style>