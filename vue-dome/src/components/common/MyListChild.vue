<template>
    <div class="grid-item">
      <slot></slot>
    </div>
</template>

<script>
  export default {
    name: "my-listchild"
  }
</script>

<style scoped lang="less">
  @rem:25rem;
  .grid-item{
    flex: 1;
    height: 100/@rem;
    min-width: 33%;
    max-width: 34%;
  }
</style>
