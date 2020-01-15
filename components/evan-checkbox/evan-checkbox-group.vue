<template>
	<view class="evan-checkbox-group">
		<slot></slot>
	</view>
</template>

<script>
	export default {
		name: 'EvanCheckboxGroup',
		props: {
			value: {
				type: Array,
				default: null
			},
			disabled: {
				type: Boolean,
				default: false
			},
			max: {
				type: Number,
				default: null
			}
		},
		watch: {
			value: {
				deep: true,
				handler(value) {
					this.deepSetValue(this.$children)
				}
			}
		},
		methods: {
			onCheckboxChange(label) {
				const value = this.value || []
				const index = value.findIndex((v) => v === label)
				if (index !== -1) {
					value.splice(index, 1)
				} else {
					value.push(label)
				}
				this.$emit('input', value)
				this.$emit('change', value)
			},
			deepSetValue(array) {
				if (Array.isArray(array)) {
					array.forEach((child) => {
						let childName = child.$options.name
						if (childName === 'EvanCheckbox') {
							if (typeof child.setValue === 'function') {
								child.setValue(this.value)
							}
						} else if (child.$children) {
							this.deepSetValue(child.$children)
						}
					})
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
</style>
