<template>
  <div class="assistive-text" :aria-live="ariaLive" aria-relevant="additions">
    <slot></slot>
    <p v-for="(text, index) in textToRead" :key="index">{{ text }}</p>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      required: true
    },
    ariaLive: {
      type: String,
      default: "polite",
      validator: value => {
        return ["assertive", "polite", "off"].indexOf(value) !== -1;
      }
    }
  },
  data() {
    return {
      textToRead: []
    };
  },
  watch: {
    value(val) {
      this.say(val);
    }
  },
  mounted() {
    this.say(this.value);
  },
  methods: {
    say(text) {
      if (text) {
        this.textToRead.push(text);
      }
    }
  }
};
</script>

<style scoped>
.assistive-text {
  overflow: hidden;
  clip: rect(0 0 0 0);
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  border: 0;
}
</style>
