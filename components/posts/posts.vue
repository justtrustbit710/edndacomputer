<template>
  <ul v-if="posts.length > 0" class="cards">
    <li v-for="(post, index) in posts" :key="index">
      <NuxtLink :to="`${postType}/${post.slug}`" class="card card--clickable">
        <template v-if="postType === 'projects'">
          <span class="flex-1">
            <h6 class="inline-block py-1 px-2 mr-1 bg-gray text-white text-sm font-medium rounded-sm">
              {{ post.category }}
            </h6>
            <h3 class="card-title">{{ post.title }}</h3>
            <p class="mt-2">{{ post.description }}</p>
          </span>
          <img v-if="post.cover" class="cover-image" :src="post.cover" />
        </template>

        <template v-else>
          <div class="px-4 py-6 mx-auto sm:max-w-xl md:max-w-full lg:max-w-screen-xl md:px-24 lg:px-8 lg:pt-8 pb-4">
            <div class="mb-10 border-t border-b divide-y">
              <div class="grid py-8 sm:grid-cols-4">
                <div class="mb-4 sm:mb-0">
                  <div class="space-y-1 text-xs font-semibold tracking-wide uppercase">
                    <a
                      href="/"
                      class="transition-colors duration-200 text-purple-400 hover:text-purple-800"
                      aria-label="Category"
                      >{{ post.category }}</a
                    >
                    <p v-if="post.createdAt" class="text-gray-600">{{ formatDate(post.createdAt) }}</p>
                  </div>
                </div>
                <div class="sm:col-span-3 lg:col-span-3">
                  <div class="mb-3">
                    <NuxtLink :to="`${postType}/${post.slug}`" class="link">
                      <p class="text-3xl font-extrabold leading-none sm:text-4xl xl:text-4xl">
                        {{ post.title }}
                      </p>
                    </NuxtLink>
                  </div>
                  <p class="text-gray-700">
                    {{ post.description }}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </template>
      </NuxtLink>
    </li>
  </ul>
  <div v-else-if="loading" class="cards">
    <div v-for="placeholder in placeholderClasses" :key="placeholder.id" class="card">
      <content-placeholders :rounded="true" :class="placeholder">
        <content-placeholders-heading />
      </content-placeholders>
    </div>
  </div>
  <p v-else class="max-w-5xl mx-auto">
    {{ amount > 1 ? 'Posts not found' : 'Post not found' }}
  </p>
</template>

<script>
export default {
  name: 'Posts',
  props: {
    postType: {
      type: String,
      default: 'blog',
      validator: (val) => ['blog', 'projects'].includes(val),
    },
    amount: {
      // ? https://content.nuxtjs.org/fetching#limitn
      type: Number,
      default: 10,
      validator: (val) => val >= 0 && val < 100,
    },
    sortBy: {
      // ? https://content.nuxtjs.org/fetching#sortbykey-direction
      type: Object,
      default: () => ({
        key: 'slug',
        direction: 'desc', // you probably want 'asc' here
      }),
      validator: (obj) => typeof obj.key === 'string' && typeof obj.direction === 'string',
    },
  },
  data() {
    return {
      posts: [],
      loading: true,
    }
  },
  computed: {
    placeholderClasses() {
      const classes = ['w-full', 'w-2/3', 'w-5/6']
      return [...Array.from({ length: this.amount }, (v, i) => classes[i % classes.length])] // repeats classes after one another
    },
  },
  async mounted() {
    this.loading = true
    this.posts = await this.fetchPosts()
    this.loading = false
  },
  methods: {
    formatDate(dateString) {
      const date = new Date(dateString)
      return date.toLocaleDateString(process.env.lang) || ''
    },
    async fetchPosts(postType = this.postType, amount = this.amount, sortBy = this.sortBy) {
      return this.$content(postType)
        .sortBy(sortBy.key, sortBy.direction)
        .limit(amount)
        .fetch()
        .catch((err) => console.error(err) || [])
    },
  },
}
</script>
