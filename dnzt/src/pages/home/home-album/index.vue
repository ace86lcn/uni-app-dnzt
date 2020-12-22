<template>
    <scroll-view scroll-y  @scrolltolower="handleToLower" class="album_view">
        <view class="swiper_warp">
            <!-- 1 自动轮播 autoplay
            2 指示器 indicator-dots 
            3 衔接轮播 circular 
            4 swiper 默认的高度 150px
            5 image 
                默认的宽度 320px => 基本样式中 重置了 100%
                默认的高度 240px 
            6 计算图片的宽度和高度的比例
            7 把图片的比例也写到swiper标签样式 
            8 swiper-item 100%
            -->
            <swiper
                autoplay
                indicator-dots
                circular
            >
                <view class="swiper_item"
                    v-for="i in banner"
                    :key="i.id"
                >
                     <image :src="i.thumb"></image>   
                </view>
            </swiper>
        </view>
        <view class="album_warp">
            <navigator class="album_item" v-for="i in album" :key="i.id" :url="`/pages/album/index?id=${i.id}`">
                <view class="album_left">
                    <view class="image_item" >
                        <image :src="i.cover" mode="aspectFill"></image>
                    </view>
                </view>
                <view class="album_info">
                    <view class="info_name">{{i.name}}</view>
                    <view class="info_desc">{{i.desc}}</view>
                    <view class="info_btn">
                        <view class="info_attention">关注</view>
                    </view>
                </view>
            </navigator>
        </view>
    </scroll-view>
</template>
<script>
export default {
    data() {
        return {
            // 请求参数
            params: {
                limit: 30,      // 显示数量
                order: "new",   // 参数
                skip: 0         // 分页
            },
            banner: [],     // 轮播图
            album: [],      // 列表数组
            hasMore: true   // 是否有分页
        }
    },
    mounted() {
        // 修改页面的标题
        uni.setNavigationBarTitle({title: '专辑'})

        this.getList()
    },
    methods: {
        // 获取数据
        getList() {
            this.request({
                url: 'http://157.122.54.189:9088/image/v1/wallpaper/album',
                data: this.params
            })
            .then((res) => {
                if (res.res.banner.length === 0) {
                    this.banner = res.res.banner
                }
                if (res.res.album.length === 0) {
                    this.hasMore = false
                    uni.showToast({
                        tile: '没有更多数据了~',
                        icon: 'none'
                    })
                    return false
                }
                this.album = [...res.res.album, ...this.album]
            })
        },
        // 上滑刷新获取分页
        handleToLower() {
            if (this.hasMore) {
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
    .album_view {
        height: calc(100vh - 36px);
    }
    .swiper_warp {
        swiper {
            // 750rpx 326.0869565217392
            // height: calc(750rpx / 2.3 );
            height: 326.1rpx;
            image {
                height: 100%;
            } 
        }
        
    }
    .album_warp {
        padding: 10rpx;
    .album_item {
        display: flex;
        padding: 10rpx 0;
        border-bottom: 1rpx solid #ccc;
        .album_left {
            .image_item {
                flex: 1;
                padding: 10rpx;
                image { 
                    width: 200rpx;
                    height: 200rpx;
                }
            }
        }
        .album_info {
            flex: 2;
             padding: 0 10rpx;
             overflow: hidden;
            .info_name {
               font-size: 30rpx;
               color: #000;
               padding: 10rpx 0;
            }

            .info_desc {
                padding: 10rpx 0;
                font-size: 24rpx;
                text-overflow: ellipsis;
                overflow: hidden;
                white-space: nowrap;
            }

            .info_btn {
                display: flex;
                justify-content:flex-end;
                .info_attention {
                    color:$color;
                    border: 1rpx solid $color;
                    padding: 10rpx;
                }
            }
        }
    }
}
</style>