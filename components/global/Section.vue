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

		<CBox
			mb="4"
		>
			<CButton
				@click="$emit(showWidget ? 'hide' : 'show')"
			>
				<CText>
					{{ showWidget ? "Hide" : "Show" }}
				</CText>
			</CButton>
		</CBox>

		<CBox>
			<CCollapse
                :is-open="showWidget"
            >
				<slot />
			</CCollapse>
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

