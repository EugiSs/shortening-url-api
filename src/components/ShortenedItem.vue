<template>
 <Transition name="fade">
	<div class="shortening__result result">
		<div class="result__link_original">{{ props.result.linkOrig }}</div>
		<div class="result__output">
			<div class="result__link_short">{{ props.result.linkShort }}</div>
			<div class="result__button" @click="copy">Copy</div>
		</div>
	</div>
 </Transition>
</template>

<script setup>
import { ref } from "vue"
import useClipboard from "vue-clipboard3"

let props = defineProps({
	result: {
		type: Object,
		required: true
	}
})

// https://github.com/JamieCurnow/vue-clipboard3
const { toClipboard } = useClipboard()
const text = ref("")

const copy = async () => {
	try {
		await toClipboard(props.result.linkShort)
	} catch (err) {
		console.log(err)
	}
}
</script>

<style lang="scss">
@import "@/assets/scss/index.scss";

.result {
	background: $White;
	border-radius: 8px;
	&__link {
		&_original {
			font-size: 16px;
			font-weight: 500;
			color: $VeryDarkViolet;
			border-bottom: 1px solid $Gray;
			text-overflow: ellipsis;
			overflow: hidden;
			white-space: nowrap;
			padding: 16px;
		}

		&_short {
			font-size: 16px;
			font-weight: 500;
			color: $Cyan;
		}
	}

	&__output {
		display: flex;
		flex-direction: column;
		gap: 16px;
		padding: 15px;
	}

	&__button {
		@include btn(100%, 40px, 16px);
		border-radius: 5px;
	}

	@media ($tablet) {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 15px;
		padding: 17px 24px;

		&__link {
			&_original {
				font-size: 20px;
				padding: 0;
				border-bottom: none;
			}
			&_short {
				font-size: 20px;
			}
		}

		&__output {
			flex-direction: row;
			align-items: center;
			gap: 24px;
			padding: 0;
		}

		&__button {
			width: 102px;
		}
	}
}
</style>
