<template>
  <!-- Text and Color is props, Header tranfer to Button -->
  <!-- @Click is a  Events -->
  <button @click="onClick()" :style="{ background: color }" class="btn">
    {{ text }}
  </button>
</template>

<script>
export default {
  name: 'Button',
  // Declare props
  props: {
    text: String,
    color: String,
  },
  methods: {
    onClick() {
      this.$emit('btn-click');
    },
  },
};
</script>
