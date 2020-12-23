<template>
    <view>
        <view class="category_tab">
            <view class="category_tab_title">
                <view class="title_inner">
                    <uni-segmented-control
                        :values="items.map(v=>v.title)"
                        :current="current"
                        @clickItem="onClickItem"
                        style-type="text"
                        active-color="#d4237a"
                    ></uni-segmented-control>
                </view>
                <view class="iconfont iconsearch"></view>
            </view>
            <scroll-view @scrolltolower="handleScrolltolower" enable-flex scroll-y class="category_tab_content">
                <view class="cate_item"
                v-for="item in vertical"
                :key="item.id"
                >
                <image :src="item.thumb" mode="widthFix" ></image>
                </view>
            </scroll-view>
        </view>
    </view>
</template>
<script>
import { uniSegmentedControl } from "@dcloudio/uni-ui"
export default {
    components: {
        uniSegmentedControl
  },
  data() {
      return {
          items: [{title: '最新', order: 'new'}, {title: '热门', order: 'hot'}],
          current: 0,
          params: {
              limit: 30,
              skip: 0,
              order: 'new'
          },
          hasMove: true,
          id: 0,
          vertical: []
      }
  },
  onLoad(options) {
      this.id = options.id
      this.getData()
  },
  methods: {
      onClickItem(e) {
        /* 
            1 根据被点击的标题 切换标题
            2 切换order 
            3 重新发送请求
        */
          if (this.current !== e.currentIndex) {
              this.current = e.currentIndex
          } else {
               // 点击的是相同的标题
              return 
          }
            this.params.order = this.items[e.currentIndex].order
            this.params.skip=0
            this.vertical=[]
            this.getData()
      },
      // 向上滑动加载图片
      handleScrolltolower() {
        if (this.hasMove) {
            this.params.skip += this.params.limit
            this.getData()
        } else {
            uni.showToast({
                 title:'没有更多数据了~',
                 icon: 'none'
             })
        }
      },
     async getData() {
         const { res } = await this.request({
             url: `http://157.122.54.189:9088/image/v1/vertical/category/${this.id}/vertical`,
             data: this.params
         })
         if (res.vertical.length === 0) {
             uni.showToast({
                 title:'没有更多数据了~',
                 icon: 'none'
             })
             return
         }
         this.vertical = [...res.vertical, ...this.vertical]
      },
      
  }
}
</script>
<style lang="scss" scoped>
.category_tab{
  .category_tab_title{
    position: relative;
    .title_inner{
      width: 60%;
      margin: 0 auto;
    }
    .iconsearch{
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      right: 5%;
    }
  }
  .category_tab_content{
    display: flex;
    flex-wrap: wrap;
    height: calc(100vh - 36px);
    .cate_item{
      width: 33.33%;
      border: 5rpx solid #fff;
      image{

      }
    }
  }
}
</style>