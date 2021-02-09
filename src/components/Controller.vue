<template>
	<div class="ctrl">
		<div class="ctrl__preview" v-if="!open" @click="openHandler">
			<p>{{ Number(+this.number).toLocaleString() }}</p>
		</div>
		<div class="ctrl__input" v-else>
			<input
				type="number"
				ref="input"
				v-model="number"
				@input="inputHandler"
				@blur="closeHandler"
				@focus="$event.target.select()"
				@submit="closeHandler"
				@keyup.enter="closeHandler"
				@keyup.esc="exitHandler"
				@keydown.tab.prevent="e => handleTabPress(e)"
			/>
			<a href="#" v-if="extra" @mousedown="extra.method">
				{{ extra.text }}
			</a>
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			getItem: { type: Function },
			extra: { type: Object },
			tab: { type: Function },
			propNum: { type: Number }
		},
		data: () => ({
			open: false,
			number: 0,
			prevNum: null
		}),
		methods: {
			openHandler() {
				this.open = true
				this.prevNum = this.number
			},
			inputHandler(e) {
				if (this.number < 0) {
					this.number = 0
				}
				if (this.getItem) {
					const item = this.getItem()
					item.number = this.number
				}
				this.$emit('getVal', e.target.value)
			},
			closeHandler() {
				if (!this.number) {
					this.number = 0
				}
				this.open = false
			},
			exitHandler(e) {
				this.number = this.prevNum
				this.open = false
			},
			handleTabPress(e) {
				const item = this.tab()
				if (!e.shiftKey) {
					item.next.open = true
				} else {
					item.prev.open = true
				}
			}
		},
		watch: {
			propNum(next, prev) {
				this.number = this.propNum
			}
		},
		updated() {
			if (this.open) {
				this.$refs.input.focus()
			}
		}
	}
</script>

<style lang="scss">
	.ctrl {
		font-size: 24px;
		margin: 15px 0;
		&__preview {
			position: relative;
			display: flex;
			align-items: center;
			cursor: pointer;
			transition: 0.3s;
			padding: 3px 15px;
			border: 1px solid #fff;
			border-radius: 3px;
			&::after {
				content: '>';
				font-size: 18px;
				transform: rotate(90deg);
				margin-left: 10px;
			}
			&:hover {
				color: #7187ff;
				border-color: #7187ff;
			}
		}
		&__input {
			position: relative;
			input {
				width: 120px;
				outline: none;
				appearance: none;
				border: 1px solid #7187ff;
				border-radius: 3px;
				padding: 3px 3px 3px 15px;
				font-size: 24px;
				background-color: transparent;
				color: #7187ff;
			}
			a {
				font-size: 12px;
				color: #7187ff;
				position: absolute;
				left: 110%;
				top: 0;
			}
		}
	}
</style>
