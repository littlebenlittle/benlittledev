<template>
	<CFlex
		minHeight="100vh"
		justify="top"
		direction="column"
	>
		<CBox
			mb="1"
			padding="4"
			minHeight="xs"
			maxHeight="lg"
			width="90vw"
			maxWidth="3xl"
			border="1px"
			borderColor="color"
			borderRadius="lg"
			overflow="scroll"
		>
			<nuxt-content
				:document="selectedSkill"
			/>
		</CBox>

			<CFlex
				justify="center"
				direction="column"
				align="center"
			>
				<CBox
					my="1"
					width="70vw"
					max-width="3xl"
				>
					<CFlex
						justify="center"
						direction="column"
					>

						<CBox
							align="left"
							my="1"
							mx="4"
						>
							<CSimpleGrid
								:columns="3"
								spacing="10px"
								minChildWidth="150px"
							>
								<CButton
									height="70px"
									maxWidth="3xs"
									padding="4"
									margin="4"
									white-space="normal"
									v-for="skill in skills"
									:key=skill.id
									v-if="!skill.draft"
									border="1px"
									borderColor="color"
									borderRadius="lg"
									align="center"
									@click="setSelectedSkill(skill)"
								>
									{{ skill.name }}
								</CButton>
							</CSimpleGrid>
						</CBox>

						<CBox
							align="left"
							my="1"
							mx="4"
							v-if="usingDevMode"
						>
							<CHeading
								as="h4"
							>
								Drafts
							</CHeading>
							<CButton>
								new
							</CButton>
						</CBox>

						<CBox
							align="left"
							my="1"
							mx="4"
							v-if="usingDevMode"
						>
							<CSimpleGrid
								:columns="3"
								spacing="10px"
								minChildWidth="150px"
							>
								<CButton
									height="70px"
									maxWidth="3xs"
									padding="4"
									margin="4"
									white-space="normal"
									v-for="skill in skills"
									:key=skill.id
									v-if="skill.draft"
									border="1px"
									borderColor="color"
									borderRadius="lg"
									align="center"
									@click="setSelectedSkill(skill)"
								>
									{{ skill.name }}
								</CButton>
							</CSimpleGrid>
						</CBox>

					</CFlex>
				</CBox>
			</CFlex>

	</CFlex>
</template>

<script>
import {
	CBox, CFlex, CHeading, CText, CSimpleGrid, CButton,
} from '@chakra-ui/vue'

export default {
	name: "AboutWidget",

	components: {
		CBox, CFlex, CHeading, CText, CSimpleGrid, CButton,
	},

	props: {
			skills: Array,
	},

	data() {
		return {
			selectedSkillId: "default",
			categories: [
				"Math",
				"Development",
				"Languages",
				"Principles",
			],
		}
	},

	computed: {
		selectedSkill: {
			get() {
				var skill
				this.skills.forEach(s => {
					if (s.id == this.selectedSkillId) {
						skill = s
					}
				})
				return skill
			},
			set(skill) {
				this.selectedSkillId = skill.id
			},
		},
		usingDevMode() {
			return window.webpackHotUpdate
		},
	},

	methods: {
		setSelectedSkill(skill) {
			this.$el.scrollIntoView({behavior: 'smooth'})
			this.selectedSkill = skill
		},
	},
}
</script>

