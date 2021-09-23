<template>
	<view
	@touchstart="handleTouchStart"
	@touchend="handleTouchEnd"
	>学习触屏事件</view>
</template>

<script>
	export default {
		data() {
			return {
				// 按下的时间
				startTime: 0,
				// 按下的坐标
				startX: 0,
				startY: 0
			}
		},
		methods: {
			handleTouchStart(event) {
				this.startTime = Date.now()
				this.startX = event.changedTouches[0].clientX
				this.startY = event.changedTouches[0].clientY
			},
			handleTouchEnd(event) {
				const endTime = Date.now()
				const endX = event.changedTouches[0].clientX
				const endY = event.changedTouches[0].clientY
				
				// 判断按下的时长
				if(endTime - this.startTime > 2000) {
					return
				}
				
				// 滑动方向
				let direction = ""
				// 先判断用户滑动的距离 是否合法 注意距离需要加上绝对值
				if(Math.abs(endX - this.startX) > 10) {
					// 滑动方向
					direction = endX - this.startX > 0 ? "right" : "left"
				} else {
					return
				}
				
				// 用户做了合法的滑动操作
				console.log(direction)
			}
		}
	}
</script>

<style lang="scss" scoped>
	view {
		width: 100%;
		height: 500rpx;
		background-color: aqua;
	}
</style>
