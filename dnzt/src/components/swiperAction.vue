<template>
    <view
        @touchstart="handleTouchstart"
        @touchend="handleTouchend"
    >
        <slot></slot>
    </view>
</template>
<script>
export default {
    data() {
        return {
            startTime: 0,
            startX: 0,
            startY: 0
        }
    },
    methods: {
        handleTouchstart(e) {
            this.startTime = Date.now()
            this.startX = e.changedTouches[0].clientX
            this.startY = e.changedTouches[0].clientY
        },
        handleTouchend(e) {
            const endtime = Date.now()
            const endX = e.changedTouches[0].clientX
            const endY = e.changedTouches[0].clientY

            // 判断按下的时长
            if (endtime - this.startTime > 2000) {
                return
            }

            // 判断方向
            let direction = ''
            // 先判断用户滑动的距离 是否合法 合法：判断滑动的方向  注意 距离要加上绝对值
            if (Math.abs(endX - this.startX) > 10 && Math.abs(endY - this.startY < 10)) {
                direction = endX - this.startX > 0 ? 'right' : 'left'
            } else {
                return
            }

            this.$emit('swiperAction', { direction })
        }
    }
}
</script>
<style lang="scss" scoped>

</style>