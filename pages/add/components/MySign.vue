<template>
  <view class="myBrush-container" ref="myBrush">
    <view class="brush_btn">
      <view class="resign" @touchend="closeBrush">取消</view>
      <text class="title">手写签名</text>
      <view class="confirm" @touchend="finish">完成</view>
    </view>
    <view class="brush_content">
      <canvas canvas-id="myCanvas" @touchstart="touchStart" @touchmove="touchMoved" @touchend="touchEnd"></canvas>
      <uni-icons  class="content_img" type="trash" color="#0E9CFF" size="30" @touchend="clearBrush" @click="clearBrush"></uni-icons>
    </view>
  </view>
</template>

<script>
export default {
  name: "MySign",
 async mounted() {
    this.init()
    // 在画板以外松开鼠标后冻结画笔
    document.onmouseup = () => {
      this.isDrawing = false
    }
  },
  props: {
    //线宽
    lineWidth: {
      type: Number,
      default: 8
    },
    //线的颜色
    lineColor: {
      type: String,
      default: '#000000'
    },
    //背景颜色
    bgColor: {
      type: String,
      default: '#FFF'
    },
  },
  data() {
    return {
      ctx:{},         //绘图图像
      points:[]       //路径点集合
    }
  },
  computed: {
    myBg() {
      return this.bgColor ? this.bgColor : 'rgba(255, 255, 255, 0)'
    }
  },
  methods: {
    init() {
      this.ctx = uni.createCanvasContext('myCanvas',this)
      this.ctx.height = this.$refs.myBrush.offsetHeight;
      this.ctx.width = this.$refs.myBrush.offsetWidth;
      this.ctx.setFillStyle(this.myBg)
      this.ctx.lineWidth = this.lineWidth;
      this.ctx.lineCap = "round"
      this.ctx.lineJoin = "round"
    },
    //触摸开始，获取到起点
    touchStart(e){
      let startX = e.changedTouches[0].x;
      let startY = e.changedTouches[0].y;
      let startPoint = {X:startX,Y:startY};

      /* **************************************************
          #由于uni对canvas的实现有所不同，这里需要把起点存起来
       * **************************************************/
      this.points.push(startPoint);

      //每次触摸开始，开启新的路径
      this.ctx.beginPath();
    },

    //触摸移动，获取到路径点
    touchMoved(e){
      let moveX = e.changedTouches[0].x;
      let moveY = e.changedTouches[0].y;
      let movePoint = {X:moveX,Y:moveY};
      this.points.push(movePoint);       //存点
      let len = this.points.length;
      if(len>=2){
        this.draw();                   //绘制路径
      }

    },

    // 触摸结束，将未绘制的点清空防止对后续路径产生干扰
    touchEnd(){
      this.points=[];
    },

    /* ***********************************************
    #   绘制笔迹
    #   1.为保证笔迹实时显示，必须在移动的同时绘制笔迹
    #   2.为保证笔迹连续，每次从路径集合中区两个点作为起点（moveTo）和终点(lineTo)
    #   3.将上一次的终点作为下一次绘制的起点（即清除第一个点）
    ************************************************ */
    draw() {
      let point1 = this.points[0]
      let point2 = this.points[1]
      this.points.shift()
      this.ctx.moveTo(point1.X, point1.Y)
      this.ctx.lineTo(point2.X, point2.Y)
      this.ctx.stroke()
      this.ctx.draw(true)
    },

    //清空画布
    clearBrush(e){
      let canvasH = this.$refs.myBrush.$el.offsetHeight;
      let canvasW = this.$refs.myBrush.$el.offsetWidth;
      this.ctx.clearRect(0, 0, canvasW, canvasH);
      this.ctx.draw(true);
    },

    //完成绘画并保存到本地
    finish(){
      uni.canvasToTempFilePath({
        canvasId: 'myCanvas',
        success: (res)=> {
          let path = res.tempFilePath;
          this.$emit('saveImage',path)
        }
      })
    },
    /**
     *
     */
    closeBrush(){
      this.$emit('closeBrush')
    }
  },
}
</script>

<style scoped lang="scss">
.myBrush-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 700rpx;
  position: fixed;
  bottom: 0;
  background-color: #fff;
  border-radius: 20rpx 20rpx 0 0;
  padding: 1%;
  box-sizing: border-box;
  z-index: 2011;

  .brush_btn {
    height: 5vh;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 2%;
    border-bottom: 1px solid #dcdfe6;
    box-sizing: border-box;

    .confirm, .resign {
      height: 80%;
      box-sizing: border-box;
      line-height: 4vh;
      color: #fff;
      border-radius: 36rpx;
    }

    .title {
      font-weight: bold;
    }

    .resign {
      background: #fff;
      color: #606266;
      width: 10%;
    }

    .confirm {
      background: #409EFF;
      width: 100rpx;
      text-align: center;
    }
  }

  .brush_content {
    width: 100%;
    height: calc(100% - 5vh);
    padding: 4%;
    box-sizing: border-box;

    canvas {
      border: 1px dashed #dcdfe6;
      height: 100%;
      width: 100%;
      box-sizing: border-box;
    }

    .content_img {
      position: absolute;
      bottom: 6%;
      right: 10%;
      width: 48rpx;
      height: 48rpx;
      z-index: 1;
    }
  }
}
</style>