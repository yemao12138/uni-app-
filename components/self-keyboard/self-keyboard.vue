<template>
	<view class="keyboard">
		<view class="row">
			<view class="value-input">
				<!-- <view class="value-input-item">{{ values }}</view> -->
				<input class="value-input-items" type="text" :value="values" disabled unselectable="on" />
			</view>
			<view class="clear-button" @click="clear">清空</view>
		</view>
		<view class="scroll-row">
			<view class="left-button">
				<uni-icons type="arrowleft" size="30" @click="scrollLeftClick"></uni-icons>
			</view>
			<view class="content">
				<scroll-view class="scroll-view_H" scroll-x="true" scroll-with-animation="true" lower-threshold="5" @scroll="scroll" @scrolltolower="scrolllower" :scroll-left="scrollLeft">
					<view v-for="item in datasource.firstrow" :key="item.id" class="content-item" @click="normal(item)">
						<view class="content-item-box">
							<view>{{ item.value }}</view>
							<view>{{ item.size }}</view>
						</view>
					</view>
				</scroll-view>
			</view>
			<view class="right-button">
				<uni-icons type="arrowright" size="30" @click="scrollRightClick"></uni-icons>
			</view>
		</view>
		<view class="key-row">
			<template v-for="item in datasource.secondrow">
				<template v-if="item.option">
					<view v-if="item.option=='cancel'" class="item item-normal" :key="item.id" @click="cannel(item)">{{ item.value }}</view>
					<view v-if="item.option=='commit'" class="item item-commit" :key="item.id" @click="commit(item)">{{ item.value }}</view>
					<view v-if="item.option=='backspace'" class="item item-normal" :key="item.id" @click="backspace(item)">
						<uni-icons type="arrowthinleft" size="30"></uni-icons>
					</view>
					<view v-if="item.option=='space'" class="item item-normal" :key="item.id" @click="space(item)">{{ item.value }}</view>
				</template>
				<template v-else>
					<view v-if="item.size==0" class="item item-num" :key="item.id" @click="num(item)">
						{{ item.value }}
					</view>
					<view v-else class="item" :key="item.id" @click="normal(item)">
						<view>{{ item.value }}</view>
						<view>{{ item.size }}</view>
					</view>
				</template>
			</template>
		</view>
	</view>
</template>

<script>
	/// 该组件需要UNI的icons组件支持
	import uniIcons from "@/components/uni-icons/uni-icons.vue"
	export default {
		name: 'SelfKeyboard',
		components: {
			uniIcons
		},
		props: {
			value: { // 传入输入值，通过$emit(input)双向绑定，父组件内实时更新
				type: String,
				default: ''
			},
			datasource: { // 传入键盘按钮数据源
				type: Object,
				default: () => { 
					return {}
				}
			}
		},
		data() {
			return {
				values: this.value, // 组件内新建变量接收父组件传进来的输入参数
				scrollLeft: 0, // 横向滚动块滚动距离
				scrollLeftMax: 999 // 横向最大滚动距离, 默认最大999px
			};
		},
		methods: {
			scrollLeftClick() {
				if(90 >= this.scrollLeft > 0) {
					this.scrollLeft = 0
				} else if(this.scrollLeft > 90) {
					this.scrollLeft -= 90
				}
			},
			scrollRightClick() {
				// 滚动距离必须小于最大滚动距离
				if(this.scrollLeft < this.scrollLeftMax){
					this.scrollLeft += 90
				}
			},
			scrolllower() {
				// 滚动到最右侧时重设横向最大滚动距离
				this.scrollLeftMax = this.scrollLeft
				console.log(this.scrollLeftMax)
			},
			scroll(e) {
				// 手动滚动也同步scrollLeft的数据
				this.scrollLeft = e.detail.scrollLeft
			},
			clear() { // 清空
				this.values = ''
			},
			cannel() { // 取消
				this.$emit('cancel', this);
			},
			commit() { // 提交
				this.$emit('commit', this);
			},
			space() { // 添加空格
				this.values += '\xa0'
			},
			backspace() { // 删除最后一位字符
				if(this.values.length > 0){
					this.values = this.values.substr(0, this.values.length-1)
				}
			},
			num(item) { // 数字按键输入
				this.values += item.value
			},
			normal(item) { // 普通按键输入
				this.values += item.value
			},
		},
		watch: {
			values(val) { // 监听输入数据变化实时更新到父组件中
				this.$emit('input', val)
			}
		}
	}
</script>

<style>
.keyboard{
	background-color: #f7f7f7;
	width: calc(100% - 10px);
	height: auto;
	padding: 5px;
}
.row{
	width: 100%;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.row .clear-button{
	width: 19%;
	height: 30px;
	line-height: 30px;
	margin: 0;
	flex: none;
	margin-left: 5px;
	background-color: #ccc;
	color: #fff;
	border: none !important;
	padding: 0;
	border-radius: 3px;
	text-align: center;
	font-size: 14px;
}
.row .value-input{
	height: 30px;
	width: 100%;
	border-radius: 3px;
	background-color: #fff;
	overflow-x: auto;
}
.row .value-input .value-input-item{
    position: relative;
	height: 29px;
	line-height: 29px;
	width: auto;
	overflow-x: auto;
}
.row .value-input .value-input-items{
     position: relative;
 	height: 29px;
 	line-height: 29px;
 	width: auto;
 	overflow-x: auto;
 }
.row .value-input .value-input-item:after {
    position: absolute;
	content: '';
	display: inline-block;
	width: 1px;
	height: 18px;
	top: 50%;
	transform: translateY(-50%);
	animation: blink 1.2s infinite steps(1, start);
}
 
@keyframes blink {
    0%, 100% {
        background-color: #000;
    }
    50% {
        background-color: transparent;
    } 
}

.scroll-row {
	width: 100%;
	padding: 5px 0;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.scroll-row .uni-icons{
	color: #6EBCFC !important;
}
.scroll-row .content {
	width: 100%;
	overflow-x: hidden;
}
.scroll-row .content .scroll-view_H {
	white-space: nowrap;
	width: 100%;
}
.scroll-row .content .content-item {
	width: 40px;
	height: 40px;
	border-radius: 50%;
	border: 1px solid #6EBCFC;
	margin-right: 5px;
	display: inline-block;
	color: #6EBCFC;
}
.scroll-row .content .content-item .content-item-box{
	width: 100%;
	height: 100%;
	font-size: 12px;
	text-align: center;
}

.key-row {
	display: flex;
	justify-content: space-between;
	align-items: center;
	flex-wrap: wrap;
	margin-bottom: -5px;
}
.key-row .item{
	width: 19%;
	height: 30px;
	font-size: 12px;
	background-color: #fff;
	border-radius: 5px;
	text-align: center;
	color: #6EBCFC;
	margin-bottom: 5px;
	padding-bottom: 3px;
}
.key-row .item-normal {
	line-height: 30px;
	color: #666 !important; 
}
.key-row .item-normal .uni-icons{
	color: #666 !important; 
}
.key-row .item-num {
	line-height: 30px;
	background-color: #6EBCFC;
	color: #fff;
}
.key-row .item-commit {
	line-height: 30px;
	background-color: #DD524D;
	color: #fff;
}
</style>
