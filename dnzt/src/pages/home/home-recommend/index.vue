<template>
    <scroll-view 
    scroll-y 
    @scrolltolower="handleToLower"
    v-if="recommends.length > 0" 
    class="recommed_view">
        <!--推荐-->
        <view class="recommend_warp">
            <view class="recommend_item"
                v-for="(i, index) in recommends"
                :key="i.id"
            >
            <go-detail :list="recommends" :index="index">
                <image :src="i.thumb" mode="widthFix" ></image>
            </go-detail>
            </view>
        </view>
        <!--月份-->
        <view class="months_wrap">
            <view class="months_title">
                <view class="title_info">
                    <view class="info_left">
                        <text>{{monthes.DD}} /</text>
                        {{monthes.MM}} 月
                    </view>
                    <view class="info_desc">{{monthes.title}}</view>
                </view>
                <view class="months_more">更多 ></view>
            </view>
            <view class="months_content">
                <view class="months_item" v-for="(i, index) in monthes.items" :key="i.id">
                    <go-detail :list="monthes" :index="index">
                        <image mode="aspectFill" :src="i.thumb + i.rule.replace('$<Height>', 360)"></image>
                    </go-detail>
                </view>
            </view>
        </view>
        <!--热门-->
        <view class="hot_warp">
            <view class="hot_title">
                <text>热门</text>
            </view>
            <view class="hot_content">
                <view class="content_item" v-for="(i, index) in hots" :key="i.id">
                    <go-detail :list="hots" :index="index">
                        <image mode="widthFix" :src="i.thumb"></image>
                    </go-detail>
                </view>
            </view>
        </view>
    </scroll-view>
</template>
<script>
import moment from 'moment'
import goDetail from '@/components/goDetail'
export default {
    components: {
        goDetail
    },
    data() {
        return {
            // 请求的数据
            params: {
                // 要获取几条
                limit: 30,
                // 关键字
                order: "hot",
                // 要跳过几条
                skip: 0
            },
            recommends: [],  // 推荐
            monthes: {},      // 月份
            hots: [],         // 热门
            hasMOre: true     // 是否还有分页
        }
    },
    mounted() {
        // 修改页面的标题
        uni.setNavigationBarTitle({title: '推荐'})
        this.getList()
    },
    methods: {
            getList() {
            this.request({
                url: 'http://157.122.54.189:9088/image/v3/homepage/vertical',
                data: this.params
            })
            .then((res) => {
                // 是否有分页
                if (res.res.vertical.length === 0) {
                    this.hasMOre = false
                    uni.showToast({
                        title: '没有更多数据了~',
                        icon: 'none'
                    })
                    return
                }

                if (this.recommends.length === 0) {
                    // 第一次请求数据
                    // 推荐数据
                    this.recommends = res.res.homepage[1].items
                    // 月份数据
                    this.monthes = res.res.homepage[2]
                    this.monthes.MM = moment(this.monthes.atime).format('MM')
                    this.monthes.DD = moment(this.monthes.atime).format('DD')
                    }
                     //热门数据
                    this.hots = [ ...res.res.vertical, ...this.hots ]
            })
        },
        
        // 滚动到底部触发
        handleToLower() {
           /* 
            1 修改参数  skip+=limit;
            2 重新发送请求 getList()
            3 请求回来成功  hots 数据的叠加 
            */
           if (this.hasMOre) {
               this.params.skip += this.params.limit
               this.getList()
           } else {
               uni.showToast({
                title: '没有更多数据了~',
                icon: 'none'
               })
           }
        }
    }
}
</script>
<style scoped lang="scss">
.recommed_view {
    // height：屏幕的高度 -  头部标题的高度
    height: calc(100vh - 36px);
}
    .recommend_warp{
        display: flex;
        flex-wrap: wrap;
        .recommend_item {
            width: 50%;
            border: 5rpx solid #fff;
        }
    }
.months_wrap {
    .months_title {
        display: flex;
        justify-content: space-between;
        padding: 20rpx;
        .title_info {
            color: $color;
            font-size: 30rpx;
            font-weight: 600;
            display: flex;
            .info_left {
                text {
                    font-size: 36rpx;
                }
            }

            .info_desc {
                font-size: 34rpx;
                color: #666;
                margin-left: 30rpx;
            }
        }

        .months_more {
            font-size: 24rpx;
            color: $color;
        }
    }
    .months_content {
        display: flex;
        flex-wrap: wrap;
        .months_item {
            width: 33.33%;
            border: 5rpx solid #fff;
        }
    }
}
.hot_warp {
  .hot_title {
      padding: 20upx;
    text {
        border-left: 20rpx solid $color;
        padding-left: 20upx;
        font-size: 34upx;
        font-weight: 600;
    }
  }

  .hot_content {
      display: flex;
      flex-wrap: wrap;
    .content_item {
        width: 33.33%;
        border: 5upx solid #fff;
    }
  }
}
</style>