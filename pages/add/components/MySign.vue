<template>
  <view class="myBrush-container" ref="myBrush">
    <view class="brush_btn">
      <view class="resign" @click="closeBrush" @touchend="closeBrush">取消</view>
      <text class="title">手写签名</text>
      <view class="confirm" :disabled="!this.hasDrew" @click="generate" @touchend="generate">完成</view>
    </view>
    <view class="brush_content">
      <canvas canvas-id="canvas" @touchstart="touchStart" @touchmove="touchMove" @touchend="touchEnd"></canvas>
      <uni-icons @touchend="resetDraw" class="content_img" type="trash" color="#0E9CFF" size="30"></uni-icons>
    </view>
  </view>
</template>

<script>
export default {
  name: "MySign",
  mounted() {
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
    //是否裁剪 在画布设定尺寸基础上裁掉四周空白部分
    isCrop: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      ctx: {},
      hasDrew: false,
      resultImg: '',
      points: [],
      canvasTxt: null,
      startX: 0,
      startY: 0,
      isDrawing: false,//是否绘制
      sratio: 1,//宽比率
    }
  },
  computed: {
    myBg() {
      return this.bgColor ? this.bgColor : 'rgba(255, 255, 255, 0)'
    }
  },
  methods: {
    init() {
      const canvas = uni.createCanvasContext('canvas')
      canvas.height = this.$refs.myBrush.offsetHeight;
      canvas.width = this.$refs.myBrush.offsetWidth;
      console.log(canvas);
      // canvas.style.background = this.myBg
      const realW = parseFloat(window.getComputedStyle(canvas).width)
      this.canvasTxt = canvas.getContext('2d')
      this.canvasTxt.scale(1 * this.sratio, 1 * this.sratio)
      this.sratio = realW / this.$refs.myBrush.offsetWidth
      this.canvasTxt.scale(1 / this.sratio, 1 / this.sratio)
    },
    // mobile
    touchStart(e) {
      e = e || event
      e.preventDefault()
      this.hasDrew = true
      if (e.touches.length === 1) {
        let obj = {
          x: e.targetTouches[0].clientX - uni.createCanvasContext('canvas').getBoundingClientRect().left,
          y: e.targetTouches[0].clientY - uni.createCanvasContext('canvas').getBoundingClientRect().top
        }
        this.drawStart(obj)
      }
    },
    touchMove(e) {
      e = e || event
      e.preventDefault()
      if (e.touches.length === 1) {
        let obj = {
          x: e.targetTouches[0].clientX - uni.createCanvasContext('canvas').getBoundingClientRect().left,
          y: e.targetTouches[0].clientY - uni.createCanvasContext('canvas').getBoundingClientRect().top
        }
        this.drawMove(obj)
      }
    },
    touchEnd(e) {
      e = e || event
      e.preventDefault()
      if (e.touches.length === 1) {
        let obj = {
          x: e.targetTouches[0].clientX - uni.createCanvasContext('canvas').getBoundingClientRect().left,
          y: e.targetTouches[0].clientY - uni.createCanvasContext('canvas').getBoundingClientRect().top
        }
        this.drawEnd(obj)
      }
    },
    // 绘制
    drawStart(obj) {
      this.startX = obj.x
      this.startY = obj.y
      this.canvasTxt.beginPath()
      this.canvasTxt.moveTo(this.startX, this.startY)
      this.canvasTxt.lineTo(obj.x, obj.y)
      this.canvasTxt.lineCap = 'round'
      this.canvasTxt.lineJoin = 'round'
      this.canvasTxt.lineWidth = this.lineWidth * this.sratio
      this.canvasTxt.stroke()
      this.canvasTxt.closePath()
      this.points.push(obj)
    },
    /**
     * 移动
     */
    drawMove(obj) {
      this.canvasTxt.beginPath()
      this.canvasTxt.moveTo(this.startX, this.startY)
      this.canvasTxt.lineTo(obj.x, obj.y)
      this.canvasTxt.strokeStyle = this.lineColor
      this.canvasTxt.lineWidth = this.lineWidth * this.sratio
      this.canvasTxt.lineCap = 'round'
      this.canvasTxt.lineJoin = 'round'
      this.canvasTxt.stroke()
      this.canvasTxt.closePath()
      this.startY = obj.y
      this.startX = obj.x
      this.points.push(obj)
    },
    /**
     *结束绘制
     */
    drawEnd(obj) {
      this.canvasTxt.beginPath()
      this.canvasTxt.moveTo(this.startX, this.startY)
      this.canvasTxt.lineTo(obj.x, obj.y)
      this.canvasTxt.lineCap = 'round'
      this.canvasTxt.lineJoin = 'round'
      this.canvasTxt.stroke()
      this.canvasTxt.closePath()
      this.points.push(obj)
      this.points.push({x: -1, y: -1})
    },
    // 操作
    async generate() {
      if (!this.hasDrew) {
        this.$message.warning(`Warning: Not Signned!`)
        return
      }
      var resImgData = this.canvasTxt.getImageData(0, 0, uni.createCanvasContext('canvas').width, uni.createCanvasContext('canvas').height)
      this.canvasTxt.globalCompositeOperation = "destination-over"
      this.canvasTxt.fillStyle = this.myBg
      this.canvasTxt.fillRect(0, 0, uni.createCanvasContext('canvas').width, uni.createCanvasContext('canvas').height)
      this.resultImg = uni.createCanvasContext('canvas').toDataURL('image/png');
      uni.createCanvasContext('canvas').toBlob(async (blobObj) => {
        const file1 = new File([blobObj], "signature.png", {
          type: blobObj.type,
          lastModified: Date.now(),
        });
        console.log(file1);
      })
      var resultImg = this.resultImg
      this.canvasTxt.clearRect(0, 0, uni.createCanvasContext('canvas').width, uni.createCanvasContext('canvas').height)
      this.canvasTxt.putImageData(resImgData, 0, 0)
      this.canvasTxt.globalCompositeOperation = "source-over"
      if (this.isCrop) {
        const crop_area = this.getCropArea(resImgData.data)
        var crop_canvas = document.createElement('canvas')
        const crop_ctx = crop_canvas.getContext('2d')
        crop_canvas.width = crop_area[2] - crop_area[0]
        crop_canvas.height = crop_area[3] - crop_area[1]
        const crop_imgData = this.canvasTxt.getImageData(...crop_area)
        crop_ctx.globalCompositeOperation = "destination-over"
        crop_ctx.putImageData(crop_imgData, 0, 0)
        crop_ctx.fillStyle = this.myBg
        crop_ctx.fillRect(0, 0, crop_canvas.width, crop_canvas.height)
        resultImg = crop_canvas.toDataURL()
        crop_canvas = null
      }
      console.log(resultImg)

    },
    /**
     * 重绘
     */
    resetDraw() {
      this.canvasTxt.clearRect(
          0,
          0,
          uni.createCanvasContext('canvas').width,
          uni.createCanvasContext('canvas').height
      )
      this.$emit('update:bgColor', '')
      uni.createCanvasContext('canvas').style.background = 'rgba(255, 255, 255, 0)'
      this.points = []
      this.hasDrew = false
      this.resultImg = ''
    },
    /**
     * 关闭画板
     */
    closeBrush() {
      this.resetDraw();
      this.$emit("closeBrush")
    },
    /**
     * 修剪区域
     * @param imgData
     * @returns {(number|number)[]}
     */
    getCropArea(imgData) {
      var topX = uni.createCanvasContext('canvas').width;
      var btmX = 0;
      var topY = uni.createCanvasContext('canvas').height;
      var btnY = 0
      for (var i = 0; i < uni.createCanvasContext('canvas').width; i++) {
        for (var j = 0; j < uni.createCanvasContext('canvas').height; j++) {
          var pos = (i + uni.createCanvasContext('canvas').width * j) * 4
          if (imgData[pos] > 0 || imgData[pos + 1] > 0 || imgData[pos + 2] || imgData[pos + 3] > 0) {
            btnY = Math.max(j, btnY)
            btmX = Math.max(i, btmX)
            topY = Math.min(j, topY)
            topX = Math.min(i, topX)
          }
        }
      }
      topX++
      btmX++
      topY++
      btnY++
      return [topX, topY, btmX, btnY]
    }
  }
}
</script>

<style scoped lang="scss">
.myBrush-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  height: 50%;
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
      width: 18%;
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
    }
  }
}
</style>