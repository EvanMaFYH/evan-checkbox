<template>
	<view class="evan-checkbox-show">
		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">基础用法</text>
		</view>
		<evan-checkbox v-model="checked">复选框</evan-checkbox>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">自定义颜色</text>
		</view>
		<evan-checkbox v-model="colorChecked" primaryColor="#00a231">自定义颜色</evan-checkbox>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">自定义形状</text>
		</view>
		<evan-checkbox v-model="shapeChecked" shape="square">自定义形状</evan-checkbox>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">自定义图标来自uni-icons</text>
		</view>
		<evan-checkbox icon="locked" v-model="iconChecked">自定义图标来自uni-icons</evan-checkbox>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">完全自定义图标</text>
		</view>
		<evan-checkbox v-model="customIconChecked">
			完全自定义图标
			<template slot="icon">
				<image v-if="customIconChecked" style="width: 20px;height: 20px;display: block;" src="/static/logo.png"></image>
				<image v-else style="width: 30px;height: 30px;display: block;" src="/static/logo.png"></image>
			</template>
		</evan-checkbox>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">自定义大小</text>
		</view>
		<evan-checkbox :iconSize="30" :titleStyle="{'font-size':'22px',color:'red'}" v-model="sizeChecked">自定义大小</evan-checkbox>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">禁用</text>
		</view>
		<evan-checkbox v-model="disabledChecked" :disabled="true">禁用选中</evan-checkbox>
		<evan-checkbox v-model="disabledUnChecked" :disabled="true">禁用不选中</evan-checkbox>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">复选框组</text>
		</view>

		<evan-checkbox-group @change="onGroupChange" v-model="color1">
			<view class="evan-checkbox-show__group-item" v-for="item in colorList" :key="item.value">
				<evan-checkbox :label="item.value">{{item.label}}</evan-checkbox>
			</view>
		</evan-checkbox-group>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">全选，反选，清空</text>
		</view>

		<evan-checkbox-group ref="cGroup" @change="onGroupChange" v-model="color4">
			<view class="evan-checkbox-show__group-item" v-for="item in colorList" :key="item.value">
				<evan-checkbox :label="item.value">{{item.label}}</evan-checkbox>
			</view>
		</evan-checkbox-group>

		<button @tap="selectAll">全选</button>
		<button @tap="selectReverse">反选</button>
		<button @tap="clearAll">清空</button>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">自定义样式列表复选框组一</text>
		</view>
		<evan-checkbox-group @change="onGroupChange" v-model="color2">
			<view @click="toggleCheckbox1(index)" class="evan-checkbox-show__list-item" v-for="(item,index) in colorList" :key="item.value">
				<text class="evan-checkbox-show__list-item__label">{{item.label}}</text>
				<evan-checkbox ref="listCheckbox1" :preventClick="true" :label="item.value">
					<template slot="icon">
						<view>
							<uni-icons v-if="color2&&color2.includes(item.value)" type="checkmarkempty" size="25" color="#108ee9"></uni-icons>
						</view>
					</template>
				</evan-checkbox>
			</view>
		</evan-checkbox-group>
		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">自定义样式列表复选框组二(最多选择三个)</text>
		</view>
		<evan-checkbox-group :max="3" @change="onGroupChange" v-model="color3">
			<view @click="toggleCheckbox2(index)" class="evan-checkbox-show__list-item" v-for="(item,index) in colorList" :key="item.value">
				<text class="evan-checkbox-show__list-item__label">{{item.label}}</text>
				<evan-checkbox ref="listCheckbox2" :preventClick="true" :label="item.value"></evan-checkbox>
			</view>
		</evan-checkbox-group>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">popup模式基本用法</text>
		</view>

		<evan-checkbox-popup1 @confirm="onConfirm" @objConfirm="onObjConfirm" @cancel="onCancel" v-model="colorPopup1"
		 :options="colorList">
			<template v-slot:trigger="{label}">
				<view>{{label||'请选择'}}</view>
			</template>
		</evan-checkbox-popup1>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">popup模式自定义颜色</text>
		</view>

		<evan-checkbox-popup2 primaryColor="#00a231" @confirm="onConfirm" v-model="colorPopup2" :options="colorList">
			<template v-slot:trigger="{label}">
				<view>{{label||'请选择'}}</view>
			</template>
		</evan-checkbox-popup2>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">popup模式限制最多选中个数</text>
		</view>

		<evan-checkbox-popup3 :max="3" @confirm="onConfirm" v-model="colorPopup3" :options="colorList">
			<template v-slot:trigger="{label}">
				<view>{{label||'请选择'}}</view>
			</template>
		</evan-checkbox-popup3>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">popup模式自定义分隔符</text>
		</view>

		<evan-checkbox-popup4 labelSeparator="/" @confirm="onConfirm" v-model="colorPopup4" :options="colorList">
			<template v-slot:trigger="{label}">
				<view>{{label||'请选择'}}</view>
			</template>
		</evan-checkbox-popup4>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">popup模式全选反选清空</text>
		</view>

		<evan-checkbox-popup5 ref="cPopup" @confirm="onConfirm" v-model="colorPopup5" :options="colorList">
			<template v-slot:trigger="{label}">
				<view>{{label||'请选择'}}</view>
			</template>
		</evan-checkbox-popup5>

		<button @tap="popupSelectAll">全选</button>
		<button @tap="popupSelectReverse">反选</button>
		<button @tap="popupClearAll">清空</button>

		<view class="evan-checkbox-show__title">
			<text class="evan-checkbox-show__title__item">popup模式配合EvanForm使用</text>
		</view>
		<evan-form ref="form" :model="info">
			<evan-form-item prop="name" label="姓名：">
				<input placeholder="请输入姓名" v-model="info.name" class="form-item-text" placeholder-class="form-item-placeholder" />
			</evan-form-item>
			<evan-form-item prop="color" label="颜色：">
				<evan-checkbox-popup v-model="info.color" :options="colorList">
					<template v-slot:trigger="{label}">
						<view class="form-item">
							<text class="form-item-text" v-if="label">{{label}}</text>
							<text class="form-item-text form-item-placeholder" v-else>请选择颜色</text>
						</view>
					</template>
				</evan-checkbox-popup>
			</evan-form-item>
		</evan-form>
		<button style="margin-top: 20px;" @click="save">提交</button>
		<!-- #ifdef APP-PLUS -->
		<button @click="goNvue">nvue页面</button>
		<!-- #endif -->
	</view>
</template>

<script>
	import EvanCheckboxPopup from '@/components/evan-checkbox-popup/evan-checkbox-popup.vue'
	import EvanCheckboxPopup1 from '@/components/evan-checkbox-popup/evan-checkbox-popup.vue'
	import EvanCheckboxPopup2 from '@/components/evan-checkbox-popup/evan-checkbox-popup.vue'
	import EvanCheckboxPopup3 from '@/components/evan-checkbox-popup/evan-checkbox-popup.vue'
	import EvanCheckboxPopup4 from '@/components/evan-checkbox-popup/evan-checkbox-popup.vue'
	import EvanCheckboxPopup5 from '@/components/evan-checkbox-popup/evan-checkbox-popup.vue'
	export default {
		components: {
			EvanCheckboxPopup,
			EvanCheckboxPopup1,
			EvanCheckboxPopup2,
			EvanCheckboxPopup3,
			EvanCheckboxPopup4,
			EvanCheckboxPopup5
		},
		data() {
			return {
				checked: true,
				colorChecked: true,
				shapeChecked: true,
				sizeChecked: true,
				maxChecked: true,
				iconChecked: true,
				customIconChecked: true,
				disabledChecked: true,
				disabledUnChecked: false,
				color1: ['green'],
				color2: ['blue'],
				color3: null,
				color4: null,
				colorPopup1: null,
				colorPopup2: null,
				colorPopup3: null,
				colorPopup4: null,
				colorPopup5: null,
				colorList: [{
						label: '红色',
						value: 'red'
					},
					{
						label: '绿色',
						value: 'green'
					},
					{
						label: '蓝色',
						value: 'blue'
					},
					{
						label: '粉色',
						value: 'pink'
					},
					{
						label: '黑色',
						value: 'black'
					},
				],
				info: {
					name: '',
					color: null
				},
				rules: {
					name: {
						required: true,
						message: '请输入姓名'
					},
					color: {
						required: true,
						message: '请选择颜色'
					}
				}
			}
		},
		onLoad() {

		},
		methods: {
			onGroupChange(e) {
				console.log(e)
			},
			goNvue() {
				uni.navigateTo({
					url: '/pages/app/app'
				})
			},
			toggleCheckbox1(index) {
				this.$refs.listCheckbox1[index].toggle()
			},
			toggleCheckbox2(index) {
				this.$refs.listCheckbox2[index].toggle()
			},
			onConfirm(e) {
				console.log('confirm')
				console.log(e)
			},
			onObjConfirm(e) {
				console.log(e)
			},
			onCancel(e) {
				console.log('cancel')
				console.log(e)
			},
			save() {
				this.$refs.form.validate((res) => {
					if (res) {
						console.log('验证通过')
					}
				})
			},
			selectAll() {
				this.$refs.cGroup.selectAll()
				console.log(this.color4)
			},
			selectReverse() {
				this.$refs.cGroup.selectReverse()
				console.log(this.color4)
			},
			clearAll() {
				this.$refs.cGroup.clearAll()
				console.log(this.color4)
			},
			popupSelectAll() {
				this.$refs.cPopup.selectAll()
				console.log(this.colorPopup5)
			},
			popupSelectReverse() {
				this.$refs.cPopup.selectReverse()
				console.log(this.colorPopup5)
			},
			popupClearAll() {
				this.$refs.cPopup.clearAll()
				console.log(this.colorPopup5)
			}
		},
		mounted() {
			this.$nextTick(() => {
				this.$refs.form.setRules(this.rules)
			})
		}
	}
</script>

<style lang="scss">
	.evan-checkbox-show {
		padding: 20px;

		&__title {
			padding: 10px 0;

			&__item {
				font-size: 16px;
				color: #333;
				font-weight: bold;
			}
		}

		&__group-item {
			margin-bottom: 10px;
		}

		&__list-item {
			display: flex;
			align-items: center;
			height: 50px;
			border-bottom: 1px solid #eee;

			&__label {
				font-size: 16px;
				color: #333;
				flex: 1;
				margin-right: 6px;
			}
		}

		.form-item {
			min-height: 90rpx;
			display: flex;
			align-items: center;
			justify-content: flex-end;
		}

		.form-item-text {
			font-size: 28rpx;
			color: #333;
			width: 100%;
			text-align: right;
		}

		.form-item-placeholder {
			font-size: 28rpx;
			color: #999;
		}

		evan-checkbox-popup {
			width: 100%;
		}

		.evan-checkbox-popup {
			width: 100%;
		}
	}
</style>
