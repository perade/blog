<template>
  <Layout :show-logo="false">
    <!-- Author intro -->
    <Author :show-title="true" />

    <!-- List posts -->
    <div class="posts">
      <PostCard v-for="edge in $page.posts.edges" :key="edge.node.id" :post="edge.node"/>
    </div>

  </Layout>
</template>

<page-query>
query Home ($page: Int) {
  posts: allPost (page: $page, perPage: 5) @paginate {
    totalCount
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
      id
      title
      slug
      date (format: "DD MMMM YYYY")
      timeToRead
      description
      path
      tags {
        id
        title
        path
      }
    }
    }
  }
}
</page-query>

<static-query>
query {
  metadata {
    siteName
    siteUrl
    siteDescription
  }
}
</static-query>

<script>
import config from '~/.temp/config.js'
import Author from '~/components/Author.vue'
import PostCard from '~/components/PostCard.vue'

export default {
  components: {
    Author,
    PostCard
  },
  metaInfo () {
    return {
      title: 'Home',
      meta: [
        {
          key: 'google-site-verification',
          name: 'google-site-verification',
          content: '1Yg-vdbPWKP6tcy7UZCVQqYSqVFQB1lwpQpK2v5CU0g'
        },
        { property: "og:type", content: 'website' },
        { property: "og:title", content: this.$static.metadata.siteName },
        { property: "og:description", content: this.$static.metadata.siteDescription },
        { property: "og:url", content: this.$static.metadata.siteUrl },
        { property: "og:image", content: this.ogImageUrl }
      ]
    }
  },
  computed: {
    config () {
      return config
    },
    ogImageUrl () {
      return `${this.config.siteUrl}/images/Author.jpg`
    }
  }
}
</script>
