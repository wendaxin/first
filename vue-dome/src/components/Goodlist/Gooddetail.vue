<template>
  <div>
    <nev-bar title="商品详情"></nev-bar>
    <my-lunbo :url="url"></my-lunbo>

    <div class="wrap">
      <div class="w100 title">{{list.title}}</div>
      <div class="w50">市场价:<s>￥{{list.hrate}}.00</s></div>
      <div class="w50">销售价:<span class="c-red">￥{{list.rate}}.00</span></div>
      <div class="w100 box">购买数量:
        <div class="bor pdg" @click="lessnum">-</div>
        <div class="pdg" v-model="num">{{num}}</div>
        <div class="bor pdg" @click="addnum">+</div>
      </div>
      <div class="b-blue">立即购买</div>
      <div class="b-red" @click="moveball">加入购物车
        <transition name="move" @after-enter="afterEnter">
          <div class="ball" v-if="hasball"></div>
        </transition>
      </div>
      <div class="w100">商品参数</div>
      <div class="w100">商品货号:{{list.art}}</div>
      <div class="w100">库存情况:{{list.num}}件</div>
      <div class="w100">上架时间:{{list.date | convertTime('YYYY-MM-DD')}}</div>
      <router-link :to="{name: 'good.imgtext',query:{id: list.id}}" class="w100 b-c-blue">
        <div>图文介绍</div>
      </router-link>
      <router-link :to="{name: 'good.comm',query:{page: '0'}}" class="w100 b-c-red">
        <div>商品评论</div>
      </router-link>
    </div>


  </div>
</template>

<script>
  import shop from '@/storage.js'

  export default {
      name: "Gooddetail",
      data(){
        return {
          url: '',
          num: 1,
          list: {},
          hasball: false
        }
      },
      created(){
        let id = this.$route.query.id
        switch (id) {
          case "1":
            this.url = "getgoodlunbo1"
            break
          case "2":
            this.url = "getgoodlunbo2"
            break
          case "3":
            this.url = "getgoodlunbo3"
            break
          case "4":
            this.url = "getgoodlunbo4"
            break
          default:
            this.url = "getgoodlunbo"
        }
        this.$axios.get('getgooddetail')
          .then(({data})=>{
            let {a,b,c,d,e} = data
            switch (id) {
              case "1":
                this.list = a
                break
              case "2":
                this.list = b
                break
              case "3":
                this.list = c
                break
              case "4":
                this.list = d
                break
              default:
                this.list = e
            }
          })
          .catch(e=>console.log(e,"商品详情获取失败"))
      },
      methods:{
        lessnum(){
          this.num>1 ? this.num-- : this.num = 1
        },
        addnum(){
          this.num++
        },
        moveball(){
          this.hasball = true
        },
        afterEnter(){
          this.hasball = false

          shop.addshop({
            id: this.list.id,
            title: this.list.title,
            rate: this.list.rate,
            img: this.list.img,
            num: this.num,
            isbuy: true
          })
        }
      }
    }
</script>

<style scoped lang="less">
  @rem:25rem;

  @keyframes move-in{
    0% {
      top: 5/@rem;
      left: 50/@rem;
      opacity: 1;
    }
    25%{
      top: 165/@rem;
      left: 50/@rem;
      transform-origin: 0/@rem 40/@rem;
      transform: rotate(0deg);
      opacity: 1;
    }
    50%{
      top: 165/@rem;
      left: 50/@rem;
      transform-origin: 0/@rem 40/@rem;
      transform: rotate(-90deg);
      opacity: 1;
    }
    51%{
      top: 185/@rem;
      left: 10/@rem;
      transform-origin: 0/@rem 40/@rem;
      transform: rotate(0deg);
      opacity: 1;
    }
    100%{
      top: 185/@rem;
      left: 10/@rem;
      transform-origin: 0/@rem 40/@rem;
      transform: rotate(-90deg);
      opacity: 1;
    }
  }
  .move-enter-active{
    animation: move-in 1s;
  }
  .wrap{
    display: flex;
    flex-flow: row wrap;
    justify-content: space-between;
    padding: 0 10/@rem;
    font-size: 12/@rem;
    line-height: 24/@rem;
    .box{
      display: flex;
      margin-bottom: 10/@rem;
    }
    .title{
      font-size: 20/@rem;
      font-weight: bolder;
      line-height: 36/@rem;
    }
    .w50{
      width: 50%;
    }
    .c-red{
      font-size: 18/@rem;
      font-weight: bolder;
      color: #ff4348;
    }
    .w100{
      width: 100%;
    }
    .bor{
      border: 1/@rem solid #979797;
      margin: 0 10/@rem;
      border-radius: 10/@rem;
      padding: 0 10/@rem;
    }
    .pdg{
      font-size: 18/@rem;
      font-weight: bolder;
    }
    .b-blue{
      background: #498fff;
      color: #ffffff;
      border-radius: 10/@rem;
      padding: 0 10/@rem;
      font-size: 18/@rem;
      line-height: 30/@rem;
    }
    .b-red{
      background: #ff4d71;
      color: #ffffff;
      border-radius: 10/@rem;
      padding: 0 10/@rem;
      font-size: 18/@rem;
      line-height: 30/@rem;
      position: relative;
      .ball{
        position: absolute;
        width: 20/@rem;
        height: 20/@rem;
        border-radius: 50%;
        background: #00ff36;
        transform-origin: 0/@rem 60/@rem;
        top: 5/@rem;
        opacity: 0;
        left: 50/@rem;
        z-index: 100;
      }
    }
    .b-c-blue{
      border: 1/@rem solid #2eb9fb;
      border-radius: 10/@rem;
      color: #2eb9fb;
      text-align: center;
      font-size: 18/@rem;
      line-height: 30/@rem;
      margin-bottom: 10/@rem;
    }
    .b-c-red{
      border: 1/@rem solid #fb1445;
      border-radius: 10/@rem;
      color: #fb1445;
      text-align: center;
      font-size: 18/@rem;
      line-height: 30/@rem;
      margin-bottom: 10/@rem;
    }
  }
</style>
