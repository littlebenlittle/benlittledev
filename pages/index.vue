<template>
	<div id="app">
		<CBox
			d="flex"
			width="100vw"
		>
			<CFlex
				justify="center"
				direction="column"
				align="center"
			>

				<TheIntro />

				<TheContactSection />

				<Section
					title="Blog"
					:id="'blog'"
					:showWidget="'blog' == shownSectionId"
					@show="show('blog')"
					@hide="hide()"
				>
					<BlogWidget
						:posts="posts"
					/>
				</Section>

			</CFlex>
		</CBox>
	</div>
</template>

<script>
import {
	CBox, CFlex, CButton,
} from '@chakra-ui/vue'

export default {
	name: "IndexPage",
	inject: [
      "$chakraColorMode",
      "$toggleColorMode",
   ],

	components: {
		CBox, CFlex, CButton,
	},

	head: {
		title: 'Ben Little',
		meta: [
		  {
			hid: 'description',
			name: 'description',
			content: 'Mathematician and Cloud-Native Software Engineer'
		  }
		],
	},

	async asyncData ({ $content }) {
		const posts = await $content('blog')
			.only(['title', 'id', 'date', 'status'])
			.where({ status: 'published' })
			.fetch()
		return {
            posts,
		}
	},

	data() {
		return {
			selectedSkillId: "default",
			shownSectionId: "",
		}
	},

	methods: {
		show(sectionId) {
			this.shownSectionId = sectionId
		},
		hide() {
			this.shownSectionId = ""
		},
	},

	watch: {
		$route(to, from) {
			let m = to.hash.match(/([\d\w]+)(.*)?/)
			if (m != null) {
				this.shownSectionId = m[1]
			}
		}
	},

}
</script>

<style>
</style>

