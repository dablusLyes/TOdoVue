<script setup>

</script>



<template>
    <button > <slot></slot> </button>
</template>

<style lang="scss" scoped>
 button {
    padding: 8px 16px;
    border: none;
    transition: all .1s;
  }
  button:hover{
    color: #41b080;
  }
  button:active{
    color: #2d7556;
  }
</style>