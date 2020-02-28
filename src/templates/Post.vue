<template>
  <Layout>
    <div class="post-title">
      <h1 class="post-title__text">
        {{ $page.post.title }}
      </h1>

      <PostMeta :post="$page.post" />

    </div>

    <div class="post content-box">
      <!-- <div class="post__header">
        <g-image alt="Cover image" v-if="$page.post.cover_image" :src="$page.post.cover_image" />
      </div> -->
      <!-- <alert v-if="postIsOlderThanOneYear" class="bg-orange-100 border-l-4 border-orange-500 text-orange-900">
        This post is over a year old, some of this information may be out of date.
      </alert> -->

      <div class="post__content" v-html="$page.post.content" />

      <div class="post__footer">
        <PostTags :post="$page.post" />
      </div>
    </div>

    <div class="post-comments">
      <!-- Add comment widgets here -->
    </div>

    <Author class="post-author" />
  </Layout>
</template>

<script>
import moment from 'moment'
import config from '~/.temp/config.js'
import slugify from '@sindresorhus/slugify'
import PostMeta from '~/components/PostMeta'
import PostTags from '~/components/PostTags'
import Author from '~/components/Author.vue'

export default {
  components: {
    Author,
    PostMeta,
    PostTags
  },
  metaInfo () {
    return {
      title: this.$page.post.title,
      meta: [
        {
          key: 'description',
          name: 'description',
          content: this.description(this.$page.post)
        },
        { property: "og:type", content: 'article' },
        { property: "og:title", content: this.$page.post.title },
        { property: "og:description", content: this.description(this.$page.post) },
        { property: "og:url", content: this.postUrl },
        { property: "og:image", content: this.ogImageUrl },
        { property: "article:published_time", content: moment(this.$page.post.date, 'DD MMMM YYYY').format('YYYY-MM-DD') }
      ]
    }
  },
  computed: {
    config () {
      return config
    },
    ogImageUrl () {
      const imagePath = `${this.config.siteUrl}${this.config.pathPrefix}/images`
      const postPath = this.$page.post.path
      const postYear = moment(this.$page.post.date, 'DD MMMM YYYY').format('YYYY')
      const postMonth = moment(this.$page.post.date, 'DD MMMM YYYY').format('MM')
      const postImage = `${postPath}.jpg` || `${postPath}.png` || ''
      
      return postImage ? `${imagePath}/${postYear}/${postMonth}/${postImage}` : `${imagePath}/Author.jpg`
    },
    postIsOlderThanOneYear () {
      const postDate = moment(this.$page.post.date, 'DD MMMM YYYY')
      return moment().diff(postDate, 'years') > 0 ? true : false
    },
    postUrl () {
      const siteUrl = this.config.siteUrl
      const pathPrefix = this.config.pathPrefix
      const postPath = this.$page.post.path

      return postPath ? `${siteUrl}${pathPrefix}/${postPath}/` : `${siteUrl}${pathPrefix}/${slugify(this.$page.post.title)}/`
    }
  },
  methods: {
    description(post, length, clamp) {
      if (post.description) {
        return post.description
      }

      length = length || 280
      clamp = clamp || ' ...'
      let text = post.content.replace(/<pre(.|\n)*?<\/pre>/gm, '').replace(/<[^>]+>/gm, '')

      return text.length > length ? `${ text.slice(0, length)}${clamp}` : text
    }
  }
}
</script>

<page-query>
query Post ($id: ID!) {
  post: post (id: $id) {
    title
    path
    date (format: "DD MMMM YYYY")
    timeToRead
    tags {
      id
      title
      path
    }
    description
    content
  }
}
</page-query>

<style lang="scss">
.post-title {
  padding: calc(var(--space) / 2) 0 calc(var(--space) / 2);
  text-align: center;
}

.post {

  &__header {
    width: calc(100% + var(--space) * 2);
    margin-left: calc(var(--space) * -1);
    margin-top: calc(var(--space) * -1);
    margin-bottom: calc(var(--space) / 2);
    overflow: hidden;
    border-radius: var(--radius) var(--radius) 0 0;

    img {
      width: 100%;
    }

    &:empty {
      display: none;
    }
  }

  &__content {
    h2:first-child {
      margin-top: 0;
    }

    // p:first-of-type {
    //   font-size: 1.2em;
    //   color: var(--title-color);
    // }

    img {
      // width: calc(100% + var(--space) * 2);
      margin: 2em auto;
      display: block;
      // max-width: none;
      max-width: 100%;
    }
  }
}

.post-comments {
  padding: calc(var(--space) / 2);

  &:empty {
    display: none;
  }
}

.post-author {
  margin-top: calc(var(--space) / 2);
}
</style>
