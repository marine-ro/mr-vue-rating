<template>
  <div>
    <MrStarRaiting :initial-rating="rating" @update:rating="updateRating"/>
  </div>
</template>

<script>
import MrStarRaiting from '@/components/MrStarRaiting.vue';

export default {
  name: 'App',
  components: {
    MrStarRaiting,
  },
  data() {
    return {
      rating: 3.25,
    };
  },
  methods: {
    updateRating(ratingValue) {
      console.log('ratingValue', ratingValue);
    },
  },
};
</script>

<style lang="scss">
</style>
