<template>
    <view>
        <view class="album_bg">
            <image mode="widthFix" :src="album.cover"></image>
            <view class="album_info">
                <view class="album_name">{{album.name}}</view>
                <view class="album_btn">关注专辑</view>
            </view>
        </view>
        <view class="album_author">
            <view class="album_author_info">
                <image
                mode="widthFix"
                :src="album.user.avatar"
                ></image>
                <view class="author_name">{{album.user.name}}</view>
            </view>
            <view class="album_author_desc">
                <text>{{album.desc}}</text>
            </view>
        </view>
        <view class="album_list">
            <view
                class="album_item"
                v-for="(item,index) in wallpaper"
                :key="item.id"
            >
            <go-detail :list="wallpaper" :index="index">
                <image
                mode="aspectFill"
                :src="item.thumb+item.rule.replace('$<Height>',360)"
                ></image>
                </go-detail>
            </view>
        </view>
    </view>
</template>
<script>
import goDetail from "@/components/goDetail";
export default {
    components: {
        goDetail
    },
    data() {
        return {
           // 请求参数
           params: {
                limit: 30,     // 请求数据条数
                order: "new",       // 关键词
                skip: 0,            // 分页
                first: 1            // 1 返回值中 有 album对象 表示第一次请求数据  0 返回值中 只有 wallpaper 数组  不是第一次请求数据
            },
            id: -1,            // id
            album: {},         // 背景部分信息
            wallpaper: [],      // 图片
            hasMore: true       // 是否分页
        }
    },
    onLoad(options) {
        this.id = options.id
        this.getList()

    },
    // 页面触底 上拉加载下一页事件
    onReachBottom() {
        if (this.hasMore) {
            this.params.first = 0
            this.params.skip += this.params.limit
            this.getList()
        } else {
            uni.showToast({
                title: '没有数据了~',
                icon: 'none'
            })
        }
    },
    methods: {
        // 获取数据
        getList() {
            this.request({
                url: 'http://157.122.54.189:9088/image/v1/wallpaper/album/${this.id}/wallpaper',
                data: this.params
            })
            .then((res) => {
                console.log(res)
                if (Object.keys(this.album).length === 0) {
                    this.album = res.res.album
                }
                if (res.res.wallpaper.length === 0) {
                    this.hasMore = false
                    uni.showToast({
                        title: '没有数据了~',
                        icon: 'none'
                    })
                    return false
                }
                this.wallpaper = [...res.res.wallpaper, ...this.wallpaper]
            })
        }
    }
}
</script>
<style lang="scss" scoped>
    .album_bg {
        position: relative;
        image {

        }

    .album_info {
        position: absolute;
        width: 100%;
        left: 0;
        bottom: 0;
        display: flex;
        justify-content: space-between;
        height: 80rpx;
        align-items: center;
        color: #fff;
        padding: 0 15rpx;
        .album_name {
            font-size: 40rpx;
        }
        .album_btn {
            background-color: $color;
            width: 152rpx;
            height: 60rpx;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10rpx;
        }
    }
}
</style>