<template>
    <view 
    @touchstart="handleTouchstart"
    @touchend="handleTouchend"
    >
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
            // console.log('按下:' + e.changedTouches[0].clientX)
            // console.log('按下:' + e.changedTouches[0].clientY)
            this.startTime = Date.now()
            this.startX = e.changedTouches[0].clientX
            this.startY = e.changedTouches[0].clientY
        },
        handleTouchend(e) {
            // console.log('松开:' + e.changedTouches[0].clientX)
            // console.log('松开:' + e.changedTouches[0].clientY)
            const endTime = Date.now()
            const endX = e.changedTouches[0].clientX
            const endY = e.changedTouches[0].clientY

            // 判断按下的时长
            if (endTime - this.startTime > 2000) {
                return 
            }

            // 滑动方向
            // 判断滑动距离是否合法
            let directionX = ''
            let directionY = ''
            try {
               if ((Math.abs(endX - this.startX) > 10) || (Math.abs(endY - this.startY) > 10)) {
                    directionX = endX - this.startX > 0 ? 'right' : 'left'
                    directionY = endY - this.startY > 0 ? 'bottom' : 'top'
                } else {
                    return
                } 
                console.log(directionX, directionY)
            } catch(e) {
                console.log('出错了~')
            }
            
            
        }
    }
}
</script>
<style scoped lang="scss">
    view {
        width: 100%;
        height: 50vh;
        background-color: aqua;
    }
</style>