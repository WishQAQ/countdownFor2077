<template>
  <div id="app">
    <div class="text">
      YOU HAVE NEW MESSAGES
    </div>
    <div class="count_box">
      <div class="day">{{d}}<span>DAY</span></div>
      <div class="time">
        <div class="hour">{{h}}</div>
        <div class="minute">{{m}}</div>
        <div class="second">{{s}}</div>
      </div>
    </div>
  </div>
</template>

<script>
  import baffle from 'baffle'
export default {
  name: 'App',
  data(){
    return {
      d:'',
      h: '',
      m: '',
      s: '',
      sum_h: ''
    }
  },
  methods: {
    countTime() {
      // 结束时间戳
      const end = Date.parse(new Date('2020-9-17 00:00:00'))
      // 当前时间戳
      const now = Date.parse(new Date())
      // 做判断当倒计时结束时都为0
      if (now >= end) {
        this.d = 0
        this.h = 0
        this.m = 0
        this.s = 0
        return
      }
      // 用结束时间减去当前时间获得倒计时时间戳
      const msec = end - now
      let d = parseInt(msec / 1000 / 60 / 60 / 24) //算出天数
      let h = parseInt(msec / 1000 / 60 / 60 % 24)//算出小时数
      let m = parseInt(msec / 1000 / 60 % 60)//算出分钟数
      let s = parseInt(msec / 1000 % 60)//算出秒数
      //给数据赋值
      this.d = d
      this.h = h > 9 ? h : '0' + h
      this.m = m > 9 ? m : '0' + m
      this.s = s > 9 ? s : '0' + s
      //定义this指向
      const that = this
      // 使用定时器 然后使用递归 让每一次函数能调用自己达到倒计时效果
      setTimeout( () => {
        that.countTime()
      }, 1000)
    },

    countText(){
      const text = baffle(".text", {  // 要混進隨機變化的文字
        characters: '░▓░ /█░░▓ ▓▒█▓░ >░▓ ▒▓▓░█ ▒░▒█ >█░ <▓▓▒ ▒░█<',
        speed: 120
      })
      text.reveal(5000);
    },

  },
  mounted () {
    this.countTime()  // 倒计时
    this.countText()  // 文字
  }


}
</script>

<style lang="less">
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  #app{
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    .count_box{
      background: url("assets/time_background.png") no-repeat;
      background-size: contain;
      width: 800px;
      height: 400px;
      .day{
        font-size:161px;
        font-weight:400;
        color:rgba(56,255,234,1);
        text-shadow:2px 2px 6px rgba(83,255,253,0.8);
        >span{
          font-size:88px;
        }
      }
      .time{
        display: flex;
        align-items: center;
        font-size:161px;
        font-weight:400;
        >div{
          color:rgba(56,255,234,1);
          text-shadow:2px 2px 6px rgba(83,255,253,0.8);
          &:not(:last-child){
            &::after{
              content: ':';
              display: inline-block;
            }
          }
        }
      }
    }
  }
</style>
