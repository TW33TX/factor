<template>
  <div class="posts-wrap">
    <div v-if="portfolioPosts" class="portfolio-posts">
      <section v-for="post in portfolioPosts" :key="post._id" class="post card">
        <component
          :is="setting(`portfolio.components.${comp}`)"
          v-for="(comp, i) in setting('portfolio.layout.index')"
          :key="i"
          :post-id="post._id"
          format="index"
        />
      </section>
    </div>
    <div v-else class="posts-not-found">
      <div class="text">
        <div class="title">{{ setting("portfolio.notFound.title") }}</div>
        <div class="sub-title">{{ setting("portfolio.notFound.subTitle") }}</div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { factorLoadingRing } from "@factor/ui"
import { setting } from "@factor/api/settings"
import { stored } from "@factor/app/store"
import { requestPostIndex } from "@factor/post/request"
import Vue from "vue"
export default Vue.extend({
  components: { factorLoadingRing },
  data() {
    return {
      postType: "portfolio",
      loading: true
    }
  },
  metaInfo() {
    const tag = this.$route.params.tag || ""
    const title = tag ? `Tag "${tag}"` : "Projects"

    const description = tag ? `Articles related to tag: ${tag}` : "Projects and more..."
    return {
      title,
      description
    }
  },
  serverPrefetch() {
    return this.getPosts()
  },
  computed: {
    tag(this: any) {
      return this.$route.params.tag || this.$route.query.tag || ""
    },
    index(this: any) {
      return stored(this.postType) || {}
    },
    portfolioPosts(this: any) {
      const { posts = [] } = this.index
      return posts
    },
    page(this: any) {
      return this.$route.query.page || 1
    },
    returnLinkText() {
      return setting("portfolio.returnLinkText") || "All Projects"
    }
  },
  watch: {
    $route: {
      handler: function(this: any) {
        this.getPosts()
      }
    }
  },
  mounted() {
    this.getPosts()
  },
  methods: {
    setting,
    async getPosts(this: any) {
      this.loading = true

      await requestPostIndex({
        postType: this.postType,
        tag: this.tag,
        status: "published",
        sort: "-date",
        page: this.page,
        limit: setting("portfolio.limit")
      })

      this.loading = false
    }
  }
})
</script>

<style lang="less">
.posts-wrap {
  .loading-entries {
    display: flex;
    justify-content: center;
    padding: 5em;
  }
}
</style>
