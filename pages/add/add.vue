<template>
  <view class="app-container">
    <my-header title="账单录入" :showBtn="true"></my-header>
    <view class="info-container" :style="{'margin-top': customBarH + 'px'}">
      <view class="container_row">
        <text class="row_title">客户名字</text>
        <text class="row_value f_bold">{{ '王庆' }}
          <text class="row_icon">{{ '>' }}</text>
        </text>
      </view>
      <view class="container_row">
        <text class="row_title">衣服款式</text>
        <text class="row_value">{{ '童装、麻花...' }}
          <text class="row_icon">{{ '>' }}</text>
        </text>
      </view>
      <view class="container_row">
        <text class="row_title">录入时间</text>
        <text class="row_value">{{ '2020-12-01' }}
          <text class="row_icon">{{ '>' }}</text>
        </text>
      </view>
      <view class="container_row">
        <text class="row_title">衣服总数</text>
        <text class="row_value">{{ '120' + '/件' }}
          <text class="row_icon">{{ '>' }}</text>
        </text>
      </view>
      <view class="container_col">
        <view class="col_title">申请备注</view>
        <view class="col_textarea">
          <textarea placeholder="在此填写备注" placeholder-class="placeholder" maxlength="200" @input="watchInput"
                    v-model="name"/>
          <view class="textarea_num">
            <text style="color: #FB231F;">{{ remnant }}</text>
            /200
          </view>
        </view>
      </view>
      <view class="container_col">
        <view class="col_title">上传图片</view>
        <view class="col_upload">
          <image class="col_img" src="https://hellouniapp.dcloud.net.cn/static/img/uni.e19b3310.png"></image>
        </view>
      </view>
      <view class="container_col">
        <view class="sign_title">
          <text>签名</text>
          <uni-icons @click="isSign=true" type="compose" color="#0E9CFF" size="30"></uni-icons>
        </view>
        <view class="col_sign">
          <image class="sign_img" src="https://hellouniapp.dcloud.net.cn/static/img/uni.png"></image>
        </view>
      </view>
    </view>
    <MySign v-if="isSign" @closeBrush="closeBrush"/>
    <view class="foot_button">
      <view class="confirm">确认</view>
    </view>
  </view>
</template>

<script>
const MySign = () => import('./components/MySign')
export default {
  components: {
    MySign
  },
  data() {
    return {
      name: '',
      remnant: 0,
      customBarH: 0,
      isSign:false
    }
  },
  created() {
    const app = getApp()
    this.customBarH = app.globalData.customBarH;
  },
  methods: {
    closeBrush(){
      this.isSign=false
    },
    watchInput(e) {
      this.remnant = e.detail.value.length
    }
  }
}
</script>

<style scoped lang="scss">
.app-container {
  background: #efeff4;

  .info-container {
    padding: 2%;
    width: 100%;
    height: calc(100vh - 90rpx - 110rpx);
    background: #fff;
    overflow: auto;
    box-sizing: border-box;

    .container_row {
      width: 100%;
      padding: 0 2%;
      min-height: 100rpx;
      display: flex;
      border-bottom: 2rpx solid #F0EEEE;
      justify-content: space-between;
      align-items: center;
      box-sizing: border-box;

      .row_title {
        width: 50%;
        padding: 30rpx 0;
        color: #333;
        font-size: 32rpx;
        font-weight: bold;
      }

      .row_value {
        width: 50%;
        display: flex;
        justify-content: flex-end;
        white-space: nowrap;

        .row_icon {
          color: #6a6a6a;
          padding-left: 1%;
          box-sizing: border-box;

        }
      }

    }

    .container_col {
      width: 100%;
      min-height: 160rpx;
      padding: 0 2%;
      display: flex;
      flex-direction: column;
      color: #101010;
      box-sizing: border-box;

      .col_title {
        padding: 30rpx 0 10rpx 0;
        color: #333;
        font-size: 32rpx;
        font-weight: bold;
        border-bottom: 2rpx solid #F0EEEE;
        box-sizing: border-box;
      }

      .col_textarea {
        width: 100%;
        height: 200rpx;
        padding: 1%;
        background: #F9F9FB;
        opacity: 1;

        uni-textarea {
          width: 100%;
          height: 86%;
        }

        .placeholder {
          font-size: 32rpx;
          color: #C4C4C4;
        }

        .textarea_num {
          font-size: 32rpx;
          color: #aaa;
          display: flex;
          justify-content: flex-end;
        }
      }

      .col_upload {
        padding: 4% 1%;
        display: flex;
        flex-grow: 1;
        justify-content: flex-start;
        box-sizing: border-box;

        .col_img {
          width: 120rpx;
          height: 120rpx;
          background-color: #eeeeee;
        }
      }

      .sign_title {
        padding: 30rpx 0 10rpx 0;
        color: #333;
        font-size: 32rpx;
        font-weight: bold;
        border-bottom: 2rpx solid #F0EEEE;
        display: flex;
        justify-content: space-between;
        align-items: center;
        box-sizing: border-box;
      }

      .col_sign {
        padding: 4% 1%;
        display: flex;
        flex-grow: 1;
        justify-content: flex-start;
        box-sizing: border-box;

        .sign_img {
          width: 240rpx;
          height: 160rpx;
          background-color: #eeeeee;
        }
      }
    }
  }

  .foot_button {
    position: fixed;
    min-height: 120rpx;
    left: 0;
    right: 0;
    bottom: 0;
    color: #fff;
    background: #fff;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;

    .confirm {
      width: 80%;
      min-height: 80rpx;
      font-size: 32rpx;
      line-height: 80rpx;
      text-align: center;
      background: linear-gradient(93deg, #F97C55 0%, #F44545 100%);
      opacity: 1;
      border-radius: 44rpx;
    }

  }
}
</style>