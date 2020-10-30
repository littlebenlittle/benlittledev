<template>
	<CBox
		my="4"
		id="contact-section"
	>
		<CFlex
			justify="center"
			direction="column"
			align="center"
		>
			<CBox
			>
				<CHeading
					as="h2"
				>
					Contact
				</CHeading>
			</CBox >
			<CBox>
				<CFlex
				>
					<CBox
						margin="1"
					>
					  <CText fontSize="xl">
					    Download Resume:
					  </CText>
					</CBox>
					<CBox
						margin="1"
						v-for="opt in downloadOptions"
						:key="opt.name"
					>
						<a :href="opt.target">
							<CText fontSize="xl">
								{{ opt.name }}
							</CText>
						</a>
					</CBox>
				</CFlex>
			</CBox>

			<CBox
			>
				<CText
					fontSize="xl"
				>
					Message me on
					<a
						href="https://keybase.io/benlittle"
					>
						 keybase
					</a>
				</CText>
			</CBox>
		</CFlex>
	</CBox>
</template>

<script>
import {
	CBox, CFlex, CHeading, CText, 
} from '@chakra-ui/vue'

export default {
	name: "TheContactSection",

	components: {
		CBox, CFlex, CHeading, CText, 
	},

	data() {
		return {
			downloadOptions: [
				{
					name: "PDF",
					target: "./resume.pdf",
				},
			],
		}
	},
}
</script>

<style>
#contact-section a {
	text-decoration: underline;
}
</style>

