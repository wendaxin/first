<template>
    <div class="detail">
      <nev-bar title="新闻详情"></nev-bar>
      <div class="title">{{content.title}}</div>
      <div class="class">
        <div class="click">{{content.click}}次点击</div>
        <div class="sort">分类:{{content.classify}}</div>
        <div class="date">添加时间:{{content.add_time}}</div>
      </div>
      <div class="content">
        {{content.content}}
      </div>
    </div>
</template>

<script>
    export default {
      name: "NewsDetail",
      data(){
        return {
          content:{}
        }
      },
      created(){
        let id = this.$route.query.id
        this.$axios.get('getnew')
          .then( ({data})=>{
            switch (id) {
              case '1':
                this.content = data.message.a
                break
              case '2':
                this.content = data.message.b
                break
              case '3':
                this.content = data.message.c
                break
              default:
                this.content = data.message.d
            }
          })
          .catch(err=>console.log(err,'新闻详情获取异常'))
      }
    }
</script>

<style scoped lang="less">
  @rem:25rem;

  .detail{
  width: 100%;
  display: flex;
  flex-flow: column;
  .title{
    font-size: 18/@rem;
    height: 36/@rem;
    line-height: 36/@rem;
    color: rgb(44, 165, 255);
    font-weight: bolder;
  }
  .class{
    height: 24/@rem;
    padding: 5/@rem 0;
    font-size: 12/@rem;
    display: flex;
    align-items: center;
    border-bottom: 1/@rem solid #a0c3bb;
    .click, .sort, .date{
      flex: auto;
    }
  }
  .content{
    font-size: 18/@rem;
    line-height: 24/@rem;
  }
}
</style>
