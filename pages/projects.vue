<template>
  <main
    class="container w-full md:max-w-xl mx-auto md:grid md:grid-cols-2 gap-2 p-4 md:p-0"
  >
    <article v-for="post in posts" :key="post.slug" class="block p-4">
      <div class="flex">
        <img class="h-10 w-10 mr-4" :src="post.icon" :alt="page.title" />
        <div>
          <h2 class="text-2xl font-extrabold hover:underline leading-tight">
            <a v-if="post.url" :href="post.url" target="_blank">
              {{ post.title }}
            </a>
            <router-link v-else :to="post.path">
              {{ post.title }}
            </router-link>
          </h2>
          <div>{{ post.description }}</div>
          <div
            class="font-normal text-xs rounded-full inline-block px-2 py-0"
            :class="getBadgeColor(poststatus)"
          >
            {{ post.status }}
          </div>
        </div>
      </div>
    </article>
  </main>
</template>

<script>
import format from 'date-fns/format'

const size = 100

const pagination = {
  getPostsOfPage($content, page) {
    return $content('projects', { deep: true })
      .without(['toc', 'body'])
      .sortBy('date', 'desc')
      .skip(size * (page - 1))
      .limit(size)
      .fetch()
  },

  async getNumberOfPages($content) {
    return Math.ceil(
      (await $content('projects', { deep: true }).only([]).fetch()).length /
        size
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
  watch: {
    async $route() {
      this.page = parseInt(this.$route.query.page || '1') || 1
      this.posts = await pagination.getPostsOfPage(this.$content, this.page)
    },
  },
  methods: {
    time(val) {
      return format(new Date(val), 'do MMMM yyyy')
    },
    getBadgeColor(status) {
      const map = {
        upcoming: 'bg-gray-300',
        'in progress': 'bg-orange-300',
        launched: 'bg-green-300',
        archived: 'bg-gray-300',
        alumni: 'bg-green-300',
      }

      return map[status] ?? ''
    },
  },
}
</script>
