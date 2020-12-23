<template>
    <view>
        <view class="category_view">
            <navigator class="category_item"
                v-for="i in category"
                :key="i.id"
                :url="`/pages/category-image/index?id=${i.id}`"
            >
                <image mode="" :src="i.cover"></image>
                <view class="category_name">{{i.name}}</view>
            </navigator>
        </view>
    </view>
</template>
<script>
export default {
    data() {
        return {
            category: []
        }
    },
    mounted() {
        // 修改页面的标题
        uni.setNavigationBarTitle({title: '分类'})
        this.getList()
    },
    methods: {
        async getList() {
            const { res } = await this.request({
                url: 'http://157.122.54.189:9088/image/v1/vertical/category'
            })
            this.category = res.category
        }
    }
}
</script>
<style scoped lang="scss">
.category_view {
    display: flex;
    flex-wrap: wrap;
  .category_item {
      width: 33.33%;
      border: 5rpx solid #fff;
      position: relative;
    image {
        height: 240rpx;
    }
    .category_name {
        position: absolute;
        left: 0;
        bottom: 0;
        color: #ffffff;
        font-size: 40rpx;
        height: 50rpx;
        // css3 渐变
        background-image: linear-gradient(to right top,rgba(0,0,0,.2),rgba(0,0,0,0));
        display: flex;
        align-items: center;
        padding-left: 20rpx;
    }
  }
}
</style>