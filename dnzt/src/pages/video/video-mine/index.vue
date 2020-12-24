<template>
    <scroll-view
    scroll-y
    enable-flex
    class="video_main"
    @scrolltolower="handleScrolltolower"
  >
    <view
      class="video_item"
      v-for="item in videowp"
      :key="item.id"
      @click="handleGoVideo(item)"
    >
      <image
        mode="widthFix"
        :src="item.img"
      ></image>
    </view>
  </scroll-view>
</template>
<script>
export default {
    props: {
        urlObj: {
            type: Object,
            default: ''
        }
    },
    data() {
        return {
            videowp: [],
            hasMove: true
        }
    },
    watch: {
        urlObj() {
            this.videowp = []
            this.getList()
        }
    },
    mounted() {
        this.getList()
    },
    methods: {
        async getList() {
            const { res } = await this.request({
                url: this.urlObj.url,
                data: this.urlObj.params
            })
            if (res.videowp.length === 0) {
                this.hasMove = false
                uni.showToast({title: '没有更多数据了~', icon: 'none'})
                return
            }
            this.videowp = [...res.videowp, ...this.videowp]
        },
        // 分页数据
        handleScrolltolower() {
            if (this.hasMove) {
                this.urlObj.params.skip += this.urlObj.params.limit
                this.getList()
            } else {
                uni.showToast({title: '没有更多数据了~', icon: 'none'})
            }
        },
        // 跳转视频播放页
        handleGoVideo(item) {
            // 1 将数据存到全局共享中
            getApp().globalData.video = item
            // 2 页面跳转
            uni.navigateTo({
                url: '/pages/videoPlay/index'
            })
        }
    }
}
</script>
<style scoped lang="scss">
.video_main {
  display: flex;
  flex-wrap: wrap;
  height: calc(100vh - 36px);
  .video_item {
    width: 33.33%;
    border: 5rpx solid #fff;
  }
}
</style>