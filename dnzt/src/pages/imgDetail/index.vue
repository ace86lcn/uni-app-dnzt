<template>
    <view>
        <!-- 用户信息 开始 -->
        <view class="user_info">
            <view class="user_icon">
                <image :src="imgDetail.user.avatar" mode="withFix"></image>
            </view>
            <view class="user_desc">
                <view class="user_name">{{imgDetail.user.name}}</view>
                <view class="user_title">{{imgDetail.cnTime}}</view>
            </view>
        </view>
        <!-- 高清大图 开始 -->
        <view class="high_img">
            <image :src="imgDetail.thumb" mode="widthFix"></image>
        </view>
        <!-- 点赞 开始 -->
        <view class="user_rank">
            <view class="rank">
                <text class="iconfont icondianzan">{{imgDetail.rank}}</text>
            </view>
            <view class="user_collect">
                <text class="iconfont iconshoucang">收藏</text>
            </view>
        </view>
    </view>
</template>
<script>
export default {
    data() {
        return {
            imgDetail: {}       // 图片信息
        }
    },
    onLoad() {
        const { imgList, imgIndex } = getApp().globalData
        this.imgDetail = imgList[imgIndex]
    }
}
</script>
<style lang="scss" scoped>
.user_info {
    display: flex;
    padding: 20rpx;
  .user_icon {
      padding: 20rpx 0;
    image {
        height: 88rpx;
        border-radius: 50%;
    }
  }

  .user_desc {
    .user_name {
        color: #000;
        font-weight: 600;
    }

    .user_title {
        color: #ccc;
        font-size: 24rpx;
        padding: 10rpx 0;
    }
  }
}
.high_img {
  image {
    border-bottom: 1rpx solid #eee;
  }
}
.user_rank {
    display: flex;
    border-bottom: 5rpx solid #eee;
    height: 80rpx;
  .rank {
      display: flex;
      justify-content: center;
      align-items: center;
      flex: 1;
  }

  .user_collect {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    .iconfont.iconshoucang {

    }
  }
}
</style>