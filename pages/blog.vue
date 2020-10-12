<template>
  <main class="container w-full md:max-w-xl mx-auto">
    <article
      v-for="post in posts"
      :key="post.slug"
      class="mb-4 block p-4 rounded"
    >
      <div class="flex justify-between text-sm">
        <div>
          <time :datetime="post.date">{{ time(post.date) }}</time>
          <span class="ml-4 text-blue-600 text-sm">{{ page.category }}</span>
        </div>
        <div class="text-gray-600 flex">
          <span v-if="post.readingTime">{{ post.readingTime.text }}</span>
        </div>
      </div>

      <h2 class="text-2xl font-extrabold hover:underline">
        <router-link :to="post.path">
          {{ post.title }}
        </router-link>
      </h2>
      <div class="flex">
        <div class="text-lg">{{ post.description }}</div>
      </div>
    </article>
    <div class="mb-16 text-center markdown">
      <router-link v-if="page > 1" :to="`/blog/?page=${page - 1}`"
        >Prev</router-link
      >
      <router-link v-if="page < pageCount" :to="`/blog/?page=${page + 1}`"
        >Next</router-link
      >
    </div>
  </main>
</template>

<script>
import format from 'date-fns/format'

const size = 10

const pagination = {
  getPostsOfPage($content, page) {
    return $content('blog', { deep: true })
      .without(['toc', 'body'])
      .sortBy('date', 'desc')
      .skip(size * (page - 1))
      .limit(size)
      .fetch()
  },

  async getNumberOfPages($content) {
    return Math.ceil(
      (await $content('blog', { deep: true }).only([]).fetch()).length / size
    )
  },
}

export default {
  async asyncData({ $content, params, error, query }) {
    const page = parseInt(query.page || '1') || 1

    const [posts, pageCount] = await Promise.all([
      pagination.getPostsOfPage($content, page),
      pagination.getNumberOfPages($content),
    ])

    return { posts, page, pageCount }
  },
  data: () => ({
    posts: [],
    page: 1,
    pageCount: 1,
  }),
  methods: {
    time(val) {
      return format(new Date(val), 'do MMMM yyyy')
    },
  },
  watch: {
    async $route() {
      this.page = parseInt(this.$route.query.page || '1') || 1
      this.posts = await pagination.getPostsOfPage(this.$content, this.page)
    },
  },
}
</script>
