<template>
  <button 
    class="btn"
    :style="{background: color}" 
    @click="onClick()"
  >
    {{ text }}
  </button>
</template>

<script>
  export default {
    name: 'ButtonVue',
    props: {
      text: String,
      color: String,
    },
    methods: {
      onClick() {
        this.$emit('btn-click')
      }
    }
  }
</script>