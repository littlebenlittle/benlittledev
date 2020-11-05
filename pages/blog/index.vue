<template>
	<BlogWidget
		:posts="posts"
	/>
</template>

<script>
import {
	CBox,
} from '@chakra-ui/vue'

export default {
	name: "BlogPage",

	components: {
		CBox,
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

}
</script>

<style>
</style>
