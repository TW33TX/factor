<template>
  <section id="news" class="page-container news-container">
    <h2 class="pretitle">{{ setting('news.pretitle') }}</h2>
    <h1 class="title">{{ setting('news.title') }}</h1>
    <news-index />
  </section>
</template>

<script lang="ts">
import { setting } from "@factor/api/settings"
import Vue from "vue"

export default Vue.extend({
  components: {
    "news-index": () => import("./news/news-index.vue")
  },
  data() {
    return {
      postType: "news",
      loading: false
    }
  },
  methods: {
    setting
  }
})
</script>

<style lang="less">
.news-container {
  background: var(--color-bg-alt);

  .pretitle {
    color: var(--color-primary);
    font-size: 1.4em;
    @media (max-width: 900px) {
      font-size: 1.2rem;
    }
  }
  .title {
    font-size: 3.2em;
    font-weight: var(--font-weight-bold);
    letter-spacing: -0.03em;
    line-height: 1.1;
    @media (max-width: 900px) {
      font-size: 2.2rem;
    }
    a {
      color: var(--color-text);
      &:hover {
        color: var(--color-primary);
      }
    }
  }

  .news-item {
    position: relative;
    height: 100%;
    padding: 3rem 2rem;
    border: 1px solid rgba(17, 16, 16, 0.1);
    border-radius: var(--border-radius);
    transition: 0.3s cubic-bezier(0.215, 0.61, 0.355, 1);

    &:hover {
      background: var(--color-white);
      color: var(--color-text);
      transform: translateY(-2px) scale(1.02);
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3), 0 15px 12px rgba(0, 0, 0, 0.22);

      .read-more {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .title {
      font-size: 1.4rem;
      font-weight: var(--font-weight-semibold);
      letter-spacing: -0.03em;
      line-height: 1.1;
      margin: 1.5rem 0 1rem;
    }
    .content {
      font-size: 1.2rem;
      padding-bottom: 1rem;
    }
    .read-more {
      position: absolute;
      left: 0;
      width: 100%;
      display: inline-block;
      padding: 0 2rem;
      font-size: 1.2em;
      opacity: 0;
      transform: translateY(1em);
      transition: 0.3s cubic-bezier(0.215, 0.61, 0.355, 1);
    }
  }
}
</style>
