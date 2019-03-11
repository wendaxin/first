<template>
    <div class="head">
      <h1 @click="goback" class="le">&nbsp<</h1>
      <h1 class="title">{{title}}</h1>
    </div>
</template>

<script>
    export default {
      name: "nev-bar",
      props:{
        title: String
      },
      data(){
        return {

        }
      },
      methods:{
        goback(){
          this.$router.go(-1)
        }
      }
    }
</script>

<style scoped lang="less">
  @rem:25rem;

  .head{
  height: 50/@rem;
  width: 100%;
  display: flex;
  .le{
    width: 20%;
    height: 100%;
    display: flex;
    align-items: center;
    font-size: 30/@rem;
  }
  .title{
    width: 60%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 30/@rem;
  }
}
</style>
