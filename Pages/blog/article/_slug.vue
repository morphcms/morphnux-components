<template>
  <div class="pt-24 w-full">
    <section class="max-w-7xl flex flex-col mx-auto">
      <content-blocks v-if="content" :content="content" class="self-center"/>
    </section>

    <section v-if="recommendedPosts.length > 0" class="grid grid-cols-3 grid-rows-1 mt-20 gap-x-10 px-56">
      <recommended-card
        v-for="(recommended, recommendedIdx) in recommendedPosts"
        :key="recommendedIdx"
        :title="recommended.title"
        :summary="recommended.summary"
        :link="'/blog/article/' + recommended.slug"
      />
    </section>
  </div>
</template>

<script>
import ContentBlocks from "../morphnux/ui/Blocks/ContentBlocks";
import ArticleHeader from "~/components/Blog/ArticleHeader";
import RecommendedCard from "~/components/Blog/RecommendedCard";
import Content from "../morphnux/modules/page-builder/models/Content";

export default {
  layout: "blog",
  components: {
    RecommendedCard,
    ArticleHeader,
    ContentBlocks,
  },
  async asyncData({$axios, params}) {
    const post = (await $axios.get(`api/blog/posts/${params.slug}`)).data;
    const recommendedPosts = (await $axios.get(`api/blog/posts/recommended/${params.slug}`)).data;
    return {
      post,
      recommendedPosts
    }
  },

  computed: {
    content() {
      if (!this.post || !this.post.content) {
        return null;
      }

      return new Content(this.post?.content);
    }
  }
}
</script>
