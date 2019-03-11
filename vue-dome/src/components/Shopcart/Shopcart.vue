<template>
  <div>
    <nev-bar title="购物车"></nev-bar>

    <ul class="list" v-if="list.length">
      <li class="wrap" v-for="item in list" :key="item.id">
        <i class="dot" v-if="!item.isbuy" @click="select(item.id)">
        </i>
        <svg class="icon" aria-hidden="true" v-else="item.isbuy" @click="select(item.id)">
          <use xlink:href="#icon-iconfontxuanzhong1"></use>
        </svg>
        <img :src="item.img" alt="" class="img"/>
        <div class="box">
          <div class="title">{{item.title}}</div>
          <div class="level">
            <div class="l">
              ￥{{item.rate}}
            </div>
            <div class="btn" @click="sub(item.id)">-</div>
            <div>{{item.num}}</div>
            <div class="btn" @click="add(item.id)">+</div>
            <span class="delete" @click="deleteitem(item.id)">删除</span>
          </div>
        </div>
      </li>
    </ul>

    <router-link :to="{name: 'good.list'}" v-else="!list.length">
      <div class="skip">空空如也</div>
    </router-link>

    <div class="close">
      <span class="w100">总计(不含运费):￥{{total}}</span>
      <span>已经选择商品{{amount}}件</span>
      <button class="btn" @click="close">去结算</button>
    </div>

  </div>
</template>

<script>
  import shop from '@/storage.js'

  export default {
    name: "Shopcart",
    data() {
      return {
        list: []
      }
    },
    created() {
      this.list = shop.getshop()
    },
    methods:{
      select(i){
        this.list = this.list.map(item=>{
          if(item.id == i){
            item.isbuy = !item.isbuy
          }
          return item
        })
      },
      sub(i){
        this.list = this.list.map(item=>{
          if(item.id == i && item.num>1){
            item.num--
          }
          return item
        })
      },
      add(i){
        this.list = this.list.map(item=>{
          if(item.id == i){
            item.num++
          }
          return item
        })
      },
      deleteitem(i){
        this.list = this.list.filter(item=>{
          if(item.id == i){
            return false
          }else {
            return true
          }
        })
      },
      close(){
        this.$toast({
          message: `你又买不起: )`,
          iconClass: 'icon icon-success',
          duration: 1500
        })
      }
    },
    computed:{
      total(){
        return this.list.reduce((add,i)=>{
          return i.isbuy ? add + i.num * i.rate : add
        },0)
      },
      amount(){
        return this.list.reduce((add,i)=>{
          return i.isbuy ? add + i.num : add
        },0)
      }
    },
    beforeRouteLeave (to, from, next) {

      if(this.list.length == 0 ||confirm("要离开这个婴儿车么?")){
        shop.setshop(this.list)
        next(true)
      }else {
        next(false)
      }

    }
  }
</script>

<style scoped lang="less">
  @rem:25rem;

  .list{
    .wrap{
      width: 100%;
      display: flex;
      flex-flow: row nowrap;
      align-items: center;
      margin-bottom: 10/@rem;
      .dot{
        width: 16/@rem;
        height: 16/@rem;
        border: 1/@rem solid #9c9c9c;
        border-radius: 50%;
        margin: 0 10/@rem;
      }
      .img{
        width: 80/@rem;
        height: 80/@rem;
        margin-right: 10/@rem;
      }
      .box{
        display: flex;
        flex-flow: column;
        font-size: 18/@rem;
        .title{
          font-size: 24/@rem;
          font-weight: bolder;
          color: #bd00ff;
          line-height: 36/@rem;
        }
        .level{
          display: flex;
          flex-flow: row nowrap;
          line-height: 30/@rem;
          .l{
            width: 90/@rem;
          }
          .btn{
            padding: 0 10/@rem;
            border: 1/@rem solid #c7c7c7;
            border-radius: 10/@rem;
            margin: 0 10/@rem;
          }
          .delete{
            color: #ff0008;
            font-size: 12/@rem;
          }
        }
      }
    }
    li:nth-last-of-type(1){
      margin-bottom: 145/@rem;
    }
  }
  .close{
    width: 100%;
    display: flex;
    position: fixed;
    bottom: 55/@rem;
    left: 0;
    flex-flow: row wrap;
    background: #b8b8b8;
    font-size: 18/@rem;
    line-height: 30/@rem;
    box-sizing: border-box;
    padding: 10/@rem;
    justify-content: space-between;
    .w100{
      width: 100%;
    }
    .btn{
      padding: 0 10/@rem;
      border-radius: 10/@rem;
      background: #ff1c23;
      color: #ffffff;
    }
  }
  .icon {
    width: 18/@rem; height: 18/@rem;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;
    margin: 0 10/@rem;
  }
  .skip{
    width: 100%;
    text-align: center;
    line-height: 200/@rem;
    font-size: 18/@rem;
    color: #111111;
  }
</style>
