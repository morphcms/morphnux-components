<template>
  <div class="flex flex-col space-y-5">
    <div class="flex flex-row justify-start items-center">
      <h2 class="text-3xl font-bold">
        <slot name="title">
        </slot>
      </h2>
    </div>
    <div class="grid grid-rows-1 grid-cols-4 gap-x-4">
      <article-card
        v-for="(article, articleIdx) in articles"
        :key="articleIdx"
        :category="article.collection"
        :title="article.title"
        :description="article.summary"
        :image="article.banner"
      />
    </div>
  </div>
</template>

<script>
import ArticleCard from "@/components/Blog/ArticleCard";

export default {
  name: "ArticleCardsList",
  components: {ArticleCard},
  props: {
    articles: {
      type: Array | Object,
      default: [],
    },
  },
}
</script>
