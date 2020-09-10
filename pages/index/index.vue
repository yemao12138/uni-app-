<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view>
			<input class="input" type="text" :value="keyValue" @focus="open()" />
		</view>
		<uni-popup ref="popup" type="bottom">
			<SelfKeyboard v-model="keyValue" :datasource="normalDatasource" @cancel="selfKeyboardCancel()" @commit="selfKeyboardCommit()"></SelfKeyboard>
		</uni-popup>
	</view>
</template>

<script>
	import uniPopup from '@/components/uni-popup/uni-popup.vue'
	import SelfKeyboard from '@/components/self-keyboard/self-keyboard.vue'
	export default {
		components: {
			uniPopup,
			SelfKeyboard
		},
		data() {
			return {
				title: 'Hello',
				keyValue: '',
				normalDatasource: {
					firstrow: [
						{ id: 'f1', value: '1', size: '1.66'},
						{ id: 'f2', value: '1', size: '1.66'},
						{ id: 'f3', value: '1', size: '1.66'},
						{ id: 'f4', value: '1', size: '1.66'},
						{ id: 'f5', value: '1', size: '1.66'},
						{ id: 'f6', value: '1', size: '1.66'},
						{ id: 'f7', value: '1', size: '1.66'},
						{ id: 'f8', value: '1', size: '1.66'}
					],
					secondrow: [
						{ id: 's1', value: '取消', size: '0', option: 'cancel' },
						{ id: 's2', value: '1', size: '1.66' },
						{ id: 's3', value: '1', size: '1.66' },
						{ id: 's4', value: '1', size: '1.66' },
						{ id: 's5', value: 'icon', size: '0', option: 'backspace' },
						{ id: 's6', value: '1', size: '1.66' },
						{ id: 's7', value: '1', size: '0' },
						{ id: 's8', value: '2', size: '0' },
						{ id: 's9', value: '3', size: '0' },
						{ id: 's10', value: '1', size: '1.66' },
						{ id: 's11', value: '1', size: '1.66' },
						{ id: 's12', value: '4', size: '0' },
						{ id: 's13', value: '5', size: '0' },
						{ id: 's14', value: '6', size: '0' },
						{ id: 's15', value: '1', size: '1.66' },
						{ id: 's16', value: '1', size: '1.66' },
						{ id: 's17', value: '7', size: '0' },
						{ id: 's18', value: '8', size: '0' },
						{ id: 's19', value: '9', size: '0' },
						{ id: 's20', value: '1', size: '1.66' },
						{ id: 's21', value: '1', size: '1.66' },
						{ id: 's22', value: '1', size: '1.66' },
						{ id: 's23', value: '0', size: '0' },
						{ id: 's24', value: '1', size: '1.66' },
						{ id: 's25', value: '1', size: '1.66' },
						{ id: 's26', value: '空格', size: '0', option: 'space' },
						{ id: 's27', value: '一', size: '1.66' },
						{ id: 's28', value: '一', size: '1.66' },
						{ id: 's29', value: '一', size: '1.66' },
						{ id: 's30', value: '确认', size: '0', option: 'commit' },
					]
				}
			}
		},
		onLoad() {
			
		},
		methods: {
			open(){
				document.activeElement.blur();
				this.$refs.popup.open()
			},
			selfKeyboardCancel(){
				if(this.$refs.popup){
					this.$refs.popup.close()
				}
			},
			selfKeyboardCommit(){
				if(this.$refs.popup){
					this.$refs.popup.close()
				}
			}
		},
		watch:{
			keyValue(val){
				console.log(val)
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200upx;
		width: 200upx;
		margin-top: 200upx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50upx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36upx;
		color: #8f8f94;
	}
	
	.input{
		border: 1px solid #ebebeb;
		width: 200px;
		height: 30px;
		border-radius: 4px;
		padding: 3px 5px;
	}
</style>
