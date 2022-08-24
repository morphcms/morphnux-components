<template>
  <div class="px-40 flex flex-col space-y-16">
    <div class="relative">
      <img src="/images/blog/5.png" alt="image" class="rounded-b-xl"/>
      <h1 class="absolute text-white text-6xl inset-y-16 inset-x-10">Sport & Wellness Blog</h1>
    </div>
    <div class="flex flex-row space-x-3 items-center">
      <p class="font-bold">More topics:</p>
      <div class="flex flex-row items-center space-x-3">
        <nuxt-link :to="'/blog/' + category.slug" v-for="(category, categoryIdx) in categories"
                   :key="categoryIdx"
                   type="customize">
          <ui-badge class="bg-white border border-1 border-gray-400 font-medium uppercase hover:border-blue-500 hover:text-blue-500">
            {{ category.name }}
          </ui-badge>
        </nuxt-link>
      </div>
    </div>
    <div class="grid grid-cols-3 gap-10">
      <post-card v-for="(article, articleIdx) in articles" :key="articleIdx"
                 :image="article.banner"
                 :title="article.title"
                 :description="article.summary"
                 :categories="article.collections"
                 :slug="article.slug"
      />
    </div>
  </div>
</template>

<script>
import PostCard from "@/components/Blog/PostCard";
import UiBadge from "../morphnux/ui/Badge";

export default {
  name: "blog",
  layout: 'blog',
  components: {
    PostCard,
    UiBadge,
  },
  async asyncData({$axios}) {
    const articles = (await $axios.get('api/blog/posts')).data;
    const categories = (await $axios.get('api/collections')).data;
    return {
      articles,
      categories,
    }
  },
  data() {
    return {
      selectedBadge: 'border-blue-500',
      notSelected: 'border-gray-400',
    }
  },
}
</script>
