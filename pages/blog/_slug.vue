<template>
  <section class="post">
    <h1 class="post__title">{{ post.fields.title }}</h1>
    <time class="post__date" :datetime="post.fields.publishDate">
      {{ postDate }}
    </time>
    <div class="post__content rendered" v-html="postContent" />
  </section>
</template>

<script>
import { documentToHtmlString } from "@contentful/rich-text-html-renderer";
export default {
  layout: "blog",
  data() {
    return {
      slug: this.$route.params.slug,
    };
  },
  computed: {
    post() {
      let post = this.$store.state.posts.filter(
        (el) => el.fields.slug === this.slug
      );
      return post[0];
    },
    postDate() {
      return new Date(this.post.fields.publishDate).toLocaleDateString(
        "en-US",
        {
          day: "2-digit",
          month: "short",
          year: "numeric",
        }
      );
    },
    postContent() {
      return documentToHtmlString(this.post.fields.body);
    },
  },
  head() {
    return {
      title: this.post.fields.title,
    };
  },
};
</script>
