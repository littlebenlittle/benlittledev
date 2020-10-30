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
		<CButton
			variant-color="blue"
			size="sm"
			class="vue-scroll-button"
			@click="scrollToSection(shownSectionId)"
			:display="'' == shownSectionId ? 'none' : ''"
		>
			Scroll to Top
		</CButton>
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
		const resume    = await $content('resume').fetch()
		const skills    = await $content('skills').fetch()
		const statement = await $content('statement').fetch()
		return {
			statement,
			resume,
            posts,
			skills,
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
		scrollToSection(sectionId) {
			let el = document.getElementById("app")
			el.scrollIntoView()
		},
	},

}
</script>

<style>
.vue-scroll-button {
	position: fixed;
	bottom: 20px;
	right: 30px;
	z-index: 99;
}

.nuxt-content h3 {
	font-size: 1.68rem;
	margin: 1.25rem 2.0rem 0 1.0rem;
}

.nuxt-content h4 {
	font-size: 1.35rem;
	margin: 0.25rem 2.0rem 0 1.0rem;
	color: gray;
}

.nuxt-content h5 {
	font-size: 1.25rem;
	color: gray;
	margin: 0.25rem 2.0rem 0 1.0rem;
}

.nuxt-content p {
	font-size: 1.25rem;
	margin: 0 2.0rem 0.5rem 1.0rem;
	text-indent: 2.0rem;
}

.nuxt-content ul {
	margin: 0.5rem 1.0rem 0.5rem 3.0rem;
}

.nuxt-content ul li {
	font-size: 1.25rem;
	margin: 0 2.0rem 0.5rem 1.0rem;
}

.nuxt-content ol {
	margin: 0.5rem 1.0rem 0.5rem 3.0rem;
}

.nuxt-content ol li {
	font-size: 1.25rem;
	margin: 0 2.0rem 0.5rem 1.0rem;
}

.nuxt-content a {
	text-decoration: underline;
	color: teal;
}

.nuxt-content blockquote {
	font-size: 1.0rem;
	color: grey;
	margin-bottom: 0.5rem;
	padding-left: 1.5rem;
}

</style>

