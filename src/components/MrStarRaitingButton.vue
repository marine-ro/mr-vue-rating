<template>
  <button
    type="button"
    class="s-rate__btn"
    :class="{'pointer': canEvent, 'disabled': disabled}"
    :style="computeButtonStyleVars"
    v-on="canEvent ? { mousemove: onStarMove, click: selected, touchstart: touchStart, touchend: touchEnd } : {}"
  >
    <span
      class="s-rate__label s-rate__label--fill"
      :style="computeLabelStyle"
      >
      <span class="s-rate__wrapp">
        <svg class="s-rate__icon" :style="computeIconStyle">
          <use :xlink:href="`#${iconref}`"></use>
        </svg>
      </span>
    </span>
    <span class="s-rate__label s-rate__label--empty">
      <span class="s-rate__wrapp">
        <svg class="s-rate__icon" :style="computeIconStyle">
          <use :xlink:href="`#${iconref}`"></use>
        </svg>
      </span>
    </span>

  </button>
</template>

<script>

export default {
  name: 'MrStarRaitingButton',
  props: {
    iconref: {
      type: String,
      required: true,
    },
    name: {
      type: String,
      default: 'rate',
    },
    size: {
      type: [Number, String],
      default: 16,
    },
    color: {
      type: String,
      default: '#dedbdb',
    },
    colorHover: {
      type: String,
      default: '#FA7921',
    },
    required: { type: Boolean },
    starId: {
      type: Number,
      required: true,
    },
    active: {
      type: Boolean,
      required: true,
    },
    fill: {
      type: Number,
      default: 0,
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
      rate: 0,
      localFill: 0,
    };
  },
  created() {
    this.localFill = this.fill;
  },
  computed: {
    computeIconStyle() {
      return {
        width: `${this.size}px`,
        height: `${this.size}px`,
      };
    },
    computeButtonStyleVars() {
      return {
        '--color-star-button': this.color,
        '--color-star-button-hover': this.colorHover,
      };
    },
    computeLabelStyle() {
      return {
        width: `${this.fill}%`,
      };
    },
    canEvent() {
      console.log('canEvent ', !(this.disabled || this.readOnly));
      return !(this.disabled || this.readOnly);
    },
  },
  methods: {
    // eslint-disable-next-line consistent-return
    onStarMove() {
      if (this.readonly) return false;
      this.over = true;
      this.$emit('star-mouse-move', this.starId);
    },
    selected() {
      this.$emit('star-selected', this.starId);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
%button-reset {
    border: none;
    margin: 0;
    padding: 0;
    width: auto;
    overflow: visible;
    outline: none;
    background: transparent;
    color: inherit;
    font: inherit;
    line-height: normal;
    -webkit-font-smoothing: inherit;
    -moz-osx-font-smoothing: inherit;
    -webkit-appearance: none;
    &:focus:not(:focus-visible) {
      outline: none;
    }
    &:focus:not(:-moz-focusring) {
      outline: none;
    }
    &::-moz-focus-inner {
    border: 0;
    padding: 0;
}
}
.s-rate {
  &__btn {
    @extend %button-reset;
    color: var(--color-star-button);
    position: relative;
    &.pointer {
      cursor: pointer;
    }
    &.disabled {
      opacity: 0.5;
    }
    // &.hover {
    //   color: var(--color-star-button-hover);
    // }
    // &.active {
    //   color: var(--color-star-button-hover);
    // }
  }
  &__label {
    &--empty {
      color: var(--color-star-button);
    }
    &--fill {
      color: var(--color-star-button-hover);
      overflow: hidden;
      z-index: 1;
      position: absolute;
      top: 0;
      left: 0;
    }
  }
  &__wrapp {
    display: flex;
    transition: transform 150ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
    pointer-events: none;
  }
  &__icon {
  flex-shrink: 0;
  user-select: none;
  stroke-width: 0;
  stroke: currentColor;
  fill: currentColor;
  display: inline-block;
}
}
</style>
