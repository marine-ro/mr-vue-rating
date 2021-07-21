/* eslint-disable array-callback-return */
<template>
  <div class="s-rate" v-if="maxRating > 0" @mouseleave="resetRating">
    <svg style="position: absolute; width: 0; height: 0;" width="0" height="0" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
      <defs>
        <symbol id="s-rate-icon-star-default" viewBox="0 0 32 32">
          <path d="M32 12.408l-11.056-1.607-4.944-10.018-4.944
          10.018-11.056 1.607 8 7.798-1.889 11.011
          9.889-5.199 9.889 5.199-1.889-11.011 8-7.798z"></path>
        </symbol>
      </defs>
    </svg>
    <MrStarRaitingButton
      v-for="(star, index) in maxRating"
      :key="star"
      :size="starSize"
      :star-id="index + 1"
      :iconref= "iconref"
      :fill="starsFill[index]"
      :active="Boolean(starsActive[index])"
      :style="computeMargin"
      :read-only="readOnly"
      :disabled="disabled"
      @star-mouse-move="setRatingOnMove"
      @star-selected="setRating"
    />
  </div>
</template>

<script>
import MrStarRaitingButton from '@/components/MrStarRaitingButton.vue';

export default {
  name: 'MrStarRaiting',
  props: {
    maxRating: {
      type: Number,
      default: 5,
    },
    initialRating: {
      type: Number,
      default: 0,
    },
    starPrecision: {
      type: Number,
      default: 1,
    },
    iconref: {
      type: String,
      default: 's-rate-icon-star-default',
    },
    starMargin: {
      type: Number,
      default: 0,
    },
    starSize: {
      type: [Number, String],
      default: 16,
    },
    readOnly: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      over: 0,
      currentRating: 0,
      selectedRating: 0,
      starsActive: [],
      starsFill: [],
    };
  },
  components: { MrStarRaitingButton },
  created() {
    this.currentRating = this.initialRating;
    this.selectedRating = this.currentRating;
    this.setCurrentStarsActive();
    this.setCurrentStarsFill();
  },
  computed: {
    computeMargin() {
      return {
        'margin-right': `${this.starMargin}px`,
      };
    },
  },
  methods: {
    setCurrentStarsActive() {
      const arr = Array.from(Array(this.maxRating), () => 0);
      this.starsActive = arr.map((_, i) => ((i + 1 <= this.currentRating) ? 1 : 0));
    },
    setRatingOnMove(starId) {
      this.currentRating = starId;
      this.setCurrentStarsFill();
    },
    setRating(starId) {
      this.currentRating = starId;
      this.selectedRating = this.currentRating;
      this.setCurrentStarsFill();
      this.$emit('update:rating', this.selectedRating);
    },
    resetRating() {
      this.currentRating = this.selectedRating;
      this.setCurrentStarsFill();
    },
    setCurrentStarsFill() {
      const arr = Array.from(Array(this.maxRating), () => 0);
      this.starsFill = arr.map((_, i) => {
        let starFill = 0;
        if (i < this.currentRating) {
          starFill = (this.currentRating - (i) > 1)
            ? 100 : (this.currentRating - (i)) * 100;
        }
        return starFill;
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.s-rate {
  display: inline-flex;
  position: relative;
}
</style>
