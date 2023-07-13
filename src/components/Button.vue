<template>
  <button 
    @click="btnClick"  
    :style="{ background: btnColor }" 
    class="btn">{{ text }}
  </button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: {
      type: String,
    },
    btnColor: {
      type: String,
      default: 'blue'
    },
  },
  methods: {
    btnClick() {
      console.log(this.text);
      this.$emit('btn-click')
    },
  },
};
</script>