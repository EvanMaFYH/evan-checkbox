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
			<text class="evan-checkbox-show__title__item">自定义样式列表复选框组一</text>
		</view>
		<evan-checkbox-group @change="onGroupChange" v-model="color2">
			<view @click="toggleCheckbox1(index)" class="evan-checkbox-show__list-item" v-for="(item,index) in colorList" :key="item.value">
				<text class="evan-checkbox-show__list-item__label">{{item.label}}</text>
				<evan-checkbox ref="listCheckbox1" :preventClick="true" :label="item.value">
					<template slot="icon">
						<view>
							<uni-icons v-if="color2&&color2.includes(item.value)" type="checkmarkempty" size="30" color="#108ee9"></uni-icons>
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
		<!-- #ifdef APP-PLUS -->
		<button @click="goNvue">nvue页面</button>
		<!-- #endif -->
	</view>
</template>

<script>
	import EvanCheckbox from '@/components/evan-checkbox/evan-checkbox.vue'
	import EvanCheckboxGroup from '@/components/evan-checkbox/evan-checkbox-group.vue'
	import UniIcons from '@/components/uni-icons/uni-icons.vue'
	export default {
		components: {
			EvanCheckbox,
			EvanCheckboxGroup,
			UniIcons
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
				]
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
			}
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
	}
</style>
