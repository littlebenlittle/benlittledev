<template>
	<CBox
		mb="4"
		id="intro"
		bg="blue.50"
		width="100vw"
		overflow="hidden"
	>
		<CBox
			align="center"
		>
			<CFlex
				padding="4"
				width="100vw"
				maxWidth="3xl"
				align="center"
				justify="center"
				direction="row"
			>
				<CSimpleGrid
					spacing="1.0rem"
					minChildWidth="13.5rem"
				>
					<CBox
					>
						<CImage
							:src="headshotImage"
							maxWidth="3xs"
							alt="Ben Litte"
						/>
					</CBox>
					<CFlex
						direction="column"
						align="left"
						justify="center"
					>
						<CBox
						>
							<CHeading
								fontSize="3xl"
								as="h1"
								text-align="left"
								my="4"
							>
								Ben Little
							</CHeading>
						</CBox>
						<CBox
						>
							<CHeading
								fontSize="2xl"
								as="h2"
								text-align="left"
								my="4"
								overflowWrap="break-word"
							>
								Mathematician and Cloud-Native Software Engineer
							</CHeading>
						</CBox>
					</CFlex>
				</CSimpleGrid>
			</CFlex>
		</CBox>
	</CBox>
</template>

<script>
import {
	CBox, CFlex, CHeading, CImage, CText, CSimpleGrid,
} from "@chakra-ui/vue"

export default {
	name: "TheIntro",

	components: {
		CBox, CFlex, CHeading, CImage, CText, CSimpleGrid,
	},

	computed: {
		headshotImage() {
			return require("@/assets/headshot.png")
		},
	}
}
</script>
