<template>
	<view class="evan-checkbox" @click="onCheckboxChange">
		<slot v-if="$slots.icon" name="icon"></slot>
		<template v-else>
			<uni-icons v-if="icon" :type="icon" :size="iconSize" :color="iconColor"></uni-icons>
			<view v-else class="evan-checkbox__inner" :class="['evan-checkbox__inner--'+shape]" :style="{width:iconSize+'px',height:iconSize+'px',backgroundColor:innerBackgroundColor,borderColor:innerBorderColor}">
				<uni-icons v-if="currentValue" type="checkmarkempty" :size="iconSize" :color="isDisabled?'#c8c9cc':'#fff'"></uni-icons>
			</view>
		</template>
		<text v-if="$slots.default" class="evan-checkbox__label" :style="mTitleStlye">
			<slot></slot>
		</text>
	</view>
</template>

<script>
	import UniIcons from '@/components/uni-icons/uni-icons.vue'
	export default {
		name: 'EvanCheckbox',
		components: {
			UniIcons
		},
		props: {
			shape: {
				type: String,
				default: 'round'
			},
			value: {
				type: Boolean,
				default: false
			},
			label: {
				type: [String, Number],
				default: null
			},
			disabled: {
				type: Boolean,
				default: false
			},
			icon: {
				type: String,
				default: null
			},
			iconSize: {
				type: Number,
				default: 20
			},
			primaryColor: {
				type: String,
				default: '#108ee9'
			},
			titleStyle: {
				type: Object,
				default: () => {
					return {}
				}
			},
			preventClick: {
				type: Boolean,
				default: false
			}
		},
		computed: {
			isGroup() {
				let parent = this.getParent()
				if (parent) {
					return true
				}
				return false
			},
			isDisabled() {
				if (this.isGroup) {
					return this.getParent().disabled || this.disabled
				}
				return this.disabled
			},
			isOverLimit() {
				if (this.isGroup) {
					let parent = this.getParent()
					if (parent.max || parent.max === 0) {
						let parentValue = parent.value || []
						if (parentValue.length >= parent.max) {
							return true
						}
					}
				}
				return false
			},
			mTitleStlye() {
				let titleStyle = Object.assign({}, this.titleStyle || {})
				let arr = Object.keys(titleStyle).map((key) => {
					if (key === 'color' && this.disabled) {
						return null
					}
					return `${key}:${titleStyle[key]}`
				}).filter((v) => v)
				return arr.join(';')
			},
			innerBackgroundColor() {
				if (this.isDisabled) {
					return '#ebedf0'
				}
				if (this.currentValue) {
					return this.primaryColor
				}
				return '#fff'
			},
			innerBorderColor() {
				if (this.isDisabled) {
					return '#c8c9cc'
				}
				if (this.currentValue) {
					return this.primaryColor
				}
				return '#c8c9cc'
			},
			iconColor() {
				if (this.isDisabled) {
					return '#ebedf0'
				}
				if (this.currentValue) {
					return this.primaryColor
				}
				return '#c8c9cc'
			}
		},
		watch: {
			value: {
				immediate: true,
				handler(value) {
					this.currentValue = value
				}
			}
		},
		data() {
			return {
				currentValue: null
			}
		},
		methods: {
			// 获取EvanCheckboxGroup组件
			getParent() {
				let parent = this.$parent
				if (parent) {
					let parentName = parent.$options.name
					while (parentName !== 'EvanCheckboxGroup') {
						parent = parent.$parent
						if (parent) {
							parentName = parent.$options.name
						} else {
							return null
						}
					}
					return parent
				}
				return null
			},
			onCheckboxChange() {
				if (!this.isDisabled && !this.preventClick && (!this.isOverLimit || this.currentValue)) {
					this.toggleValue()
				}
			},
			toggle() {
				if (!this.isDisabled && (!this.isOverLimit || this.currentValue)) {
					this.toggleValue()
				}
			},
			toggleValue() {
				this.currentValue = !this.currentValue
				this.$emit('input', this.currentValue)
				this.$emit('change', this.currentValue)
				let parent = this.getParent()
				if (parent) {
					parent.onCheckboxChange(this.label)
				}
			},
			setValue(groupValue) {
				groupValue = groupValue || []
				this.currentValue = groupValue.includes(this.label)
			}
		},
		created() {
			let parent = this.getParent()
			if (parent) {
				this.setValue(parent.value)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.evan-checkbox {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
	}

	.evan-checkbox__label {
		font-size: 16px;
		margin-left: 8px;
		color: #333;
	}

	.evan-checkbox__inner {
		border-width: 1px;
		border-style: solid;
		background-color: #fff;
		/* #ifndef APP-NVUE */
		display: flex;
		box-sizing: border-box;
		/* #endif */
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.evan-checkbox__inner--round {
		border-radius: 50%;
	}
</style>
