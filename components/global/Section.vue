<template>
	<CFlex
		my="4"
		ref="resume"
		overflow="scroll"
		justify="center"
		direction="column"
		align="center"
	>
		<CBox
		   margin="4"
		>
			<CHeading
				as="h2"
			>
				{{ title }}
			</CHeading>
		</CBox>

		<CBox>
			<slot />
		</CBox>

	</CFlex>
</template>

<script>
import {
	CBox, CFlex, CHeading, CText, CButton, CCollapse,
} from '@chakra-ui/vue'

export default {
	name: "Section",

	components: {
		CBox, CFlex, CHeading, CText, CButton, CCollapse,
	},

	props: {
		title: String,
        showWidget: {
            type:    Boolean,
            default: false,
        },
	},
}
</script>

