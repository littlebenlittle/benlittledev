<template>
	<CBox
	 id="statement-section"
	 maxWidth="3xl"
	 width="100vw"
	>
		<CBox
			id="research"
			padding="4"
		>
			<CFlex
				direction="column"
				justify="center"
				align="center"
			>
				<CBox
					margin="4"
				>
					<CHeading
						as="h2"
						text-align="center"
						mt="4"
					>
						Research
					</CHeading>
				</CBox>
				<CBox
					mb="4"
				>
					<CButton
						@click="toggleShowResearchStatement()"
					>
						<CText
						>
						{{ showResearchStatement ? "Hide" : "Show" }}
						</CText>
					</CButton>
				</CBox>
				<CBox>
					<CCollapse
						:is-open="showResearchStatement"
					>
						<nuxt-content
							:document="content"
						/>
					</CCollapse>
				</CBox>
			</CFlex>
		</CBox>
	</CBox>
</template>

<script>
import moment from 'moment'
import {
	CBox, CFlex, CText, CHeading, CButton, CCollapse,
} from '@chakra-ui/vue'

export default {
	name: "TheResearchSection",
	components: {
		CBox, CFlex, CText, CHeading, CButton, CCollapse,
	},
	props: {
		content: Object,
	},
	data() {
		return {
			showResearchStatement: false
		}
	},
	methods: {
		toggleShowResearchStatement() {
			if (!this.showResearchStatement) {
				document.getElementById('research').scrollIntoView({behavior: 'smooth'})
			}
			this.showResearchStatement = !this.showResearchStatement
		},
	},
}
</script>

