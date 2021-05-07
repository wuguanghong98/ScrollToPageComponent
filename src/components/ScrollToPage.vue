<template>
  <div id="scroll-wrap">
    <!-- 代码 开始 -->
    <section class="section-wrap" :class="test" ref="wrap" @mousewheel="mousewheelEvent">
      <div class="section"  v-for="(page,index) in btnNum" :class="generateClass(index)">
        <div class="content" :class="{active: currentIndex == index}">
          <slot :name="index + 1">
            <p class="tit">CSS3 纵向滚屏翻页，支持键盘，鼠标滚轮操作 内容： {{index + 1}}</p>
          </slot>
        </div>
      </div>
    </section>
    <ul class="section-btn">
      <!--<li class="on"></li>-->
      <!--<li></li>-->
      <!--<li></li>-->
      <!--<li></li>-->
      <!--<li></li>-->
      <li v-for="(i,index) in btnNum"
          :key="index"
          :class="{on: currentIndex == index}"
          ref="btn"
          @click="btnClick(index)"></li>
    </ul>
    <div class="arrow" ref="arrow" @click="arrowClick">&laquo;</div>
  </div>
</template>

<script>
  export default {
    name: "ScrollToPage",
    props: {
      // 向下图标颜色
      arrowColor: {
        type: String,
        default: '#000'
      },
      //右边图标背景颜色
      liBackground: {
        type: String,
        default: '#BD362F'
      },
      //右边图标激活颜色
      liOnColor: {
        type: String,
        default: '#fff'
      },
      //右边图标数量
      btnNum: {
        type: Number,
        default: 5
      }
    },
    data() {
      return {
        i: 0,
        btn: null,
        wrap: null,
        currentIndex: 0,
        test: '',
        isRun: true,
      }
    },
    mounted() {
      this.btn = this.$refs.btn
      this.wrap = this.$refs.wrap
      document.addEventListener('keydown',this.k)
      this.setStyle()
    },
    methods: {
      up(){
        this.i++;
        if(this.i == this.btn.length){
          this.i=0
        }
      },
      down(){
        this.i--;
        if(this.i < 0){
          this.i = this.btn.length-1
        }
      },
      run(){
        this.currentIndex = this.i
        this.test = 'put-section-' + this.i
      },
      //右侧列表点击方法
      btnClick(index) {
        if(this.isRun) {
          this.i = index;
          this.run();
          this.setIsRun()
        }

      },
      //向下图标点击方法
      arrowClick() {
        if(this.isRun) {
          this.up();
          this.run();
          this.setIsRun()
        }

      },
      //鼠标滚轮滚动方法
      mousewheelEvent(event) {
        if(this.isRun){
          // console.log(event);
          if(event.deltaY > 0) {
            this.up()
          } else{
            this.down()
          }
          this.run();
          this.setIsRun()
        }
      },
      //键盘上下方向键按下方法
      k(event){
        if(this.isRun) {
          let e=event||window.event;
          let key=e.keyCode||e.which||e.charCode;
          switch(key)	{
            case 38:
              this.down();
              this.run();
              break;
            case 40:
              this.up();
              this.run();
              break;
          }
          this.setIsRun()
        }
      },
      //设置是否可运行状态方法
      setIsRun() {
        this.isRun = false
        setTimeout(() => {
          this.isRun = true
        },1500)
      },
      //设置一些内容的相关颜色
      setStyle() {
        // console.log(this.$refs.arrow);
        this.$refs.arrow.style.setProperty('--arrowColor', this.arrowColor)
        // console.log(this.$refs.btn);
        this.$refs.btn.forEach((el) => {
          el.style.setProperty('--background', this.liBackground)
          el.style.setProperty('--onColor', this.liOnColor)
        })

      },
      //动态生成class
      generateClass(index) {
        return 'section-' + (index + 1)
      }
    }
  }
</script>

<style scoped lang="scss">
  ol,ul{list-style:none;}


  #scroll-wrap {
    height: 100%;
  .section-wrap{
    width:100%;
    height:100%;
    overflow:visible;
    transition:transform 1s cubic -bezier(0.86,0,0.03,1);
    -webkit-transition:-webkit-transform 1s cubic-bezier(0.86,0,0.03,1);

  .section{
    position:relative;
    width:100%;
    height:100%;
    background-position:center center;
    background-repeat:no-repeat;

  .content{
    width:100%;
    position:absolute;
    /*top:10%;*/
    color:#fff;
    /*font-size:2.4em;*/
    /*text-align:center;*/
    /*p{*/
      /*padding:0 4%;*/
      opacity:0;
    transform:translateY(25px);
    -webkit-transform:translateY(25px);
    /*}*/
  }
  .content.active {
    /*.tit{*/
      opacity:1;
      transform:translateY(0px);
      -webkit-transform:translateY(0px);
      transition:all 2s cubic-bezier(0.86,0,0.8,1);
      -webkit-transition:all 2s cubic-bezier(0.86,0,0.8,1);
    /*}*/
  }
  }
  .section-1{ background-color:#337ab7}
  .section-2{ background-color:#5cb85c}
  .section-3{ background-color:#5bc0de}
  .section-4{ background-color:#f0ad4e}
  .section-5{ background-color:#d9534f}
  }
  /*翻滚作用*/
  .put-section-0{ transform:translateY(0);-webkit-transform:translateY(0);}
  .put-section-1{ transform:translateY(-100%);-webkit-transform:translateY(-100%);}
  .put-section-2{ transform:translateY(-200%);-webkit-transform:translateY(-200%);}
  .put-section-3{ transform:translateY(-300%);-webkit-transform:translateY(-300%);}
  .put-section-4{ transform:translateY(-400%);-webkit-transform:translateY(-400%);}

  .section-btn{
    width:14px;
    position:fixed;
    right:4%;
    top:50%;
  li{
    width:14px;
    height:14px;
    cursor:pointer;
    text-indent:-9999px;
    border-radius:50%;
    -webkit-border-radius:50%;
    margin-bottom:12px;
    background:var(--background);
    text-align:center;
    color:#fff;
    onsor:pointer;
  }
  li.on{
    background:var(--onColor)
  }
  }
  }
  .arrow{
    opacity:1;
    animation: arrow 3s cubic-bezier(0.5,0,0.1,1) infinite;
    -webkit-animation:arrow 3s cubic-bezier(0.5,0,0.1,1) infinite;
    transform:rotate(-90deg);-webkit-transform:rotate(-90deg);
    position:absolute;
    bottom:10px;
    left:50%;
    margin-left:-30px;
    width:60px;
    height:60px;
    border-radius:100%;
    -webkit-border-radius:100%;
    line-height:60px;
    text-align:center;
    font-size:20px;
    cursor:pointer;
    overflow:hidden;
    color: var(--arrowColor);
    border: 1px solid var(--arrowColor)
  }
  .arrow:hover{
    animation-play-state:paused;
    -webkit-animation-play-state:paused;
  }
  @keyframes arrow{ %0,%100{bottom:10px; opacity:1;} 50%{bottom:50px; opacity:.5} }
  @-webkit-keyframes arrow{ %0,%100{bottom:10px; opacity:1;} 50%{bottom:50px; opacity:.5} }
</style>