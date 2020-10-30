<template>
	<CBox
	 id="blog-preview-section"
	>
		<CHeading
			fontSize="3xl"
			as="h2"
			text-align="center"
			my="4"
		>
			Latest from the blog
		</CHeading>
		<article
			v-for="post in posts"
			:key="post.id"
		>
			<nuxt-link :to="'/blog#' + post.id">
				<CBox
					my="4"
					px="4"
					border="1px"
					borderRadius="md"
					borderColor="color"
				>
					<CHeading
						fontSize="2xl"
						as="h3"
						text-align="center"
						my="4"
					>
						<CText>
							{{ post.title | titleCase}}
						</CText>
					</CHeading>
					<CHeading
						fontSize="md"
						as="h4"
						text-align="center"
						my="4"
					>
						<CText>
							{{ post.date | fmtDate }}
						</CText>
					</CHeading>
				</CBox>
			</nuxt-link>
		</article>
	</CBox>
</template>

<script>
import moment from 'moment'
import {
	CBox, CHeading, CText,
} from '@chakra-ui/vue'

export default {
	name: "TheBlogPreview",
	components: {
		CBox, CHeading, CText,
	},
	props: {
		posts: Array,
	},
	filters: {
		fmtDate(val) {
			return moment(val).add(1, 'day').format('MMM Do, YYYY')
		},
		titleCase(val) {
			return val.replace(
				/\w\S*/g,
				word => {
					if (word.length < 4) {
						return word
					}
					return word.charAt(0).toUpperCase() + word.substr(1).toLowerCase();
				}
			);
		},
	},
}
</script>

<style>
#blog-preview-section a {
	text-decoration: none;
}
</style>

