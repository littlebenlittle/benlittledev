<template>
	<CFlex
		justify="center"
	>
		<CBox
			padding="4"
			width="100vw"
			maxWidth="900px"
		>
			<nuxt-content :document="post" />
		</CBox>
		<CFlex
			class="gobackbutton"
			direction="column"
			align="right"
		>
			<CButton
			    margin="0.5rem"
				variant-color="blue"
				size="sm"
				@click="$router.push('/')"
			>
				Back to Home
			</CButton>
		</CFlex>
	</CFlex>
</template>

<script>
import {
	CBox, CFlex, CButton,
} from '@chakra-ui/vue'

export default {
	name: "BlogPost",

	components: {
		CBox, CFlex, CButton,
	},

	async asyncData({ $content, params }) {
		let post = await $content('blog')
			.where({ id: params.id, status: 'published' })
			.fetch()
			.catch( err => {
				error({ statusCode: 404, message: 'Post not found' })
			})
			.then( rep => {
				return rep[0]
			})
		return { post }
	},

}
</script>

<style>
.gobackbutton {
	position: fixed;
	bottom: 20px;
	right: 30px;
	z-index: 99;
}

.nuxt-content p {
	font-size: 1.25rem;
	margin: 1.0rem 0.75rem 0 0.75rem;
	/*text-indent: 2.5rem;*/
}

.nuxt-content ol, ul {
	margin: 0.25rem 0.75rem 0.25rem 1.75rem;
	width: 90%;
	font-size: 1.25rem;
	display: flex;
	flex-direction: column;
	align-content: center;
	list-style-position: outside;
}

.nuxt-content ol li, ul li {
	margin: 0.25rem 0.75rem 0.25rem 1.75rem;
}

.nuxt-content code {
	font-size: 1.125rem;
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
    margin-top: 1.5rem;
	font-size: 2.5rem;
}

.nuxt-content h2 {
    margin-top: 1.5rem;
	font-size: 2.0rem;
}

.nuxt-content h3 {
    margin-top: 1.5rem;
	font-size: 1.75rem;
	color: gray;
}

.nuxt-content h4 {
    margin-top: 1.5rem;
	font-size: 1.0rem;
	color: grey;
}

.nuxt-content h5 {
    margin-top: 1.5rem;
	font-size: 1.0rem;
	color: grey;
}
</style>
