<template>
	<CFlex
		direction="column"
		align="center"
	>
		<CFlex
			direction="column"
		>
			<CFlex
				direction="column"
				align="left"
			>
				<BlogPostSnippetBox
					v-for="post in displayedPosts"
					:key="post.id"
					margin="2"
					padding="2"
					:title="post.title"
					:id="post.id"
					:date="post.date"
					:selected="selectedPost.id == post.id"
					@select="selectPost(post.id)"
				/>
			</CFlex>
			<PageButtons
				:posts="posts"
				:currentPage="currentPage"
				:pageMax="pageMax"
				@next-page="nextPage"
				@prev-page="prevPage"
				v-if="posts.length > postsPerPage"
			/>
		</CFlex>
		<CBox
			padding="4"
			width="100vw"
			maxWidth="800px"
			height="1000px"
			overflow="scroll"
		>
			<nuxt-content :document="selectedPost" />
		</CBox>
	</CFlex>
</template>

<script>
import moment from 'moment'
import {
	CBox, CFlex,
} from '@chakra-ui/vue'

function headingValidator(val) {
	return [ "h1", "h2", "h3", "h4", "h5", "h6" ].indexOf(val) != -1
}

export default {
	name: "BlogWidget",

	components: {
		CBox, CFlex,
	},

	props: {
		posts: {
			type: Array,
		},
		postsPerPage: {
			default: 4,
			type: Number,
		},
		titleHeading: {
			default: "h3",
			headingValidator,
		},
		dateHeading: {
			default: "h4",
			headingValidator,
		},
	},

	data() {
		return {
			selectedPost: {},
			currentPage: 1,
			pageMin: 1,
		}
	},

	methods: {
		async selectPost(postId) {
			let post = await this.$content('blog')
				.where({ id: postId })
				.fetch()
				.catch( err => {
					error({ statusCode: 404, message: 'Post not found' })
				})
				.then( rep => {
					return rep[0]
				})
			this.selectedPost = post
		},
		nextPage() {
			this.currentPage = Math.min(this.pageMax, this.currentPage + 1)
		},
		prevPage() {
			this.currentPage = Math.max(this.pageMin, this.currentPage - 1)
		},
	},

	computed: {
		pageMax() {
			return Math.floor(this.displayedPosts.length / this.postsPerPage) + 1
		},
		displayedPosts() {
			let posts = this.posts.sort( (a, b) => {
				return a.date <= b.date
			})
			let start = this.postsPerPage * (this.currentPage - 1)
			let end   = start + this.postsPerPage 
			return posts.slice(start, end)
		},
	},

}
</script>

<style>
.nuxt-content p {
	font-size: 1.25rem;
	margin-bottom: 0.75rem;
	text-indent: 2.5rem;
}

.nuxt-content ol, ul {
	margin: 0.25rem 0.75rem 0.25rem 1.75rem;
	width: 90%;
	font-size: 1.5rem;
	display: flex;
	flex-direction: column;
	align-content: center;
	list-style-position: outside;
}

.nuxt-content ol li, ul li {
	margin: 0.25rem 0.75rem 0.25rem 1.75rem;
}

.nuxt-content code {
	font-size: 1.25rem;
	border-radius: 7px;
	background-color: #f2f2f2;
}

.nuxt-content li code, p code {
	padding: 0 0.5rem 0 0.5rem;
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

.nuxt-content blockquote {
	font-size: 1.0rem;
	color: grey;
	margin-bottom: 0.5rem;
	padding-left: 1.5rem;
}

.nuxt-content h1 {
	font-size: 2.5rem;
}

.nuxt-content h2 {
    margin-top: 1.5rem;
	font-size: 2.0rem;
}

.nuxt-content h3 {
	font-size: 1.2rem;
}

.nuxt-content h4 {
	font-size: 1.0rem;
	color: grey;
}

.nuxt-content h5 {
	font-size: 1.0rem;
}

</style>
