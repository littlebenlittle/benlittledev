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
				About
			</CHeading>
		</CBox>

		<CBox
			mb="4"
		>
			<CButton
				@click="toggleShowWidget()"
			>
				<CText>
					{{ showWidget ? "Hide" : "Show" }}
				</CText>
			</CButton>
		</CBox>

		<CBox>
			<CCollapse :is-open="showWidget">
				<AboutWidget :skills="skills" />
			</CCollapse>
		</CBox>

	</CFlex>
</template>

<script>
import {
	CBox, CFlex, CHeading, CText, CButton, CCollapse,
} from '@chakra-ui/vue'

export default {
	name: "TheAboutSection",

	components: {
		CBox, CFlex, CHeading, CText, CButton, CCollapse,
	},

	props: {
			skills: Array,
	},

	data() {
		return {
			showWidget: false
		}
	},

	methods: {
		toggleShowWidget() {
			this.showWidget = !this.showWidget
		},
	},
}
</script>

