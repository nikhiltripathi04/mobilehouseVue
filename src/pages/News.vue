<template>
  <section class="container mx-auto px-4 py-10">
    <div class="flex items-end justify-between mb-6">
      <h2 class="title">Latest mobile news</h2>
      <button
        class="text-xs border border-black rounded-full px-3 py-1"
        @click="reload"
        :disabled="loading"
      >
        {{ loading ? 'Refreshing…' : 'Refresh' }}
      </button>
    </div>

    <!-- Loading skeletons -->
    <div v-if="loading" class="grid md:grid-cols-3 gap-6">
      <div v-for="n in 6" :key="n" class="rounded-2xl border p-4 animate-pulse">
        <div class="h-40 rounded-xl bg-gray-100 mb-3"></div>
        <div class="h-4 bg-gray-100 rounded w-3/4 mb-2"></div>
        <div class="h-3 bg-gray-100 rounded w-1/2 mb-4"></div>
        <div class="h-3 bg-gray-100 rounded w-full mb-2"></div>
        <div class="h-3 bg-gray-100 rounded w-5/6"></div>
      </div>
    </div>

    <!-- Error -->
    <div v-else-if="error" class="rounded-xl border p-6 bg-red-50">
      <p class="text-sm">
        Couldn’t load news right now. <button class="underline" @click="reload">Try again</button>.
      </p>
    </div>

    <!-- Articles -->
    <div v-else class="grid md:grid-cols-3 gap-6">
      <article
        v-for="(a, i) in articles"
        :key="i"
        class="rounded-2xl border overflow-hidden flex flex-col"
      >
        <img
          v-if="a.image"
          :src="a.image"
          :alt="a.title"
          class="w-full h-44 object-cover"
          loading="lazy"
        />
        <div class="p-5 flex flex-col grow">
          <p class="text-xs opacity-60 mb-1">
            {{ a.source || '—' }} • {{ formatDate(a.publishedAt) }}
          </p>
          <h3 class="lg:text-3xl text-lg leading-snug mb-2 line-clamp-2">
            {{ a.title }}
          </h3>
          <p class="desc text-sm opacity-80 line-clamp-3 mb-4">
            {{ a.description }}
          </p>
          <a
            :href="a.url"
            target="_blank"
            rel="noopener"
            class="mt-auto inline-flex items-center gap-2 text-sm border border-black rounded-full px-4 py-2 hover:bg-black hover:text-white transition"
          >
            Read article
            <i class="las la-external-link-alt text-base"></i>
          </a>
        </div>
      </article>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'

type Article = {
  title: string
  url: string
  source?: string
  description?: string
  image?: string
  publishedAt?: string
}

// Props: backend endpoint that returns normalized articles
const props = withDefaults(defineProps<{
  endpoint?: string
}>(), {
  endpoint: '/api/news/mobile' // <- change if your backend route differs
})

const loading = ref(true)
const error = ref<string | null>(null)
const articles = ref<Article[]>([])

async function load() {
  loading.value = true
  error.value = null
  try {
    const res = await fetch(props.endpoint, { cache: 'no-store' })
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    const data = await res.json()
    // Expecting: { articles: Article[] }
    articles.value = Array.isArray(data?.articles) ? data.articles : []
  } catch (e: any) {
    error.value = e?.message || 'Failed to fetch news'
  } finally {
    loading.value = false
  }
}

function reload() {
  load()
}

function formatDate(iso?: string) {
  if (!iso) return ''
  const d = new Date(iso)
  return d.toLocaleDateString(undefined, { month: 'short', day: 'numeric' })
}

onMounted(load)
</script>

<style scoped>
.title { @apply text-3xl md:text-5xl font-semibold; }
.desc { @apply leading-relaxed; }
/* line clamp utility if you don’t have plugin */
.line-clamp-2 { display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
.line-clamp-3 { display: -webkit-box; -webkit-line-clamp: 3; -webkit-box-orient: vertical; overflow: hidden; }
</style>
