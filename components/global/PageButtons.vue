<template>
	<CFlex
		direction="column"
		align="center"
		justify="flex-end"
	>
		<CFlex
			direction="row"
		>
			<CButton
				@click="$emit('prev-page')"
			>
			  prev
			</CButton>
			<CButton
				@click="$emit('next-page')"
			>
			  next
			</CButton>
		</CFlex>
		<CText
		>
			Page {{ currentPage }} of {{ pageMax }}
		</CText>
	</CFlex>
</template>

<script>
import {
	CFlex, CButton, CText,
} from '@chakra-ui/vue'

export default {
	name: "PageButtons",

	components: {
		CFlex, CButton,
	},

	props: {
		pageMax: Number,
		currentPage: Number,
	},

}

</script>

<style>
</style>
