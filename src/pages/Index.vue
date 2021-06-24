<template>
  <Layout>

    <!-- Page Header -->
    <header class="masthead" 
      :style="{backgroundImage: `url(${GRIDSOME_API_URL}${general.cover.url})`}">
    <div class="overlay"></div>
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div class="site-heading">
            <h1>{{ general.title }}</h1>
            <span class="subheading">{{ general.subtitle }}</span>
          </div>
        </div>
      </div>
    </div>
    </header>
  
    <!-- Main Content -->
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div class="post-preview" v-for="edge in $page.posts.edges" :key="edge.node.id">
            <g-link :to="'/post/' + edge.node.id">
            <h2 class="post-title">
              {{edge.node.title}}
            </h2>
            </g-link>
            <p class="post-meta">Posted by
              {{edge.node.created_by.firstname + edge.node.created_by.lastname}}
              on {{edge.node.created_at}}</p>
            <span v-for="tag in edge.node.tags" :key="tag.id">
              <g-link :to="'/tag/' + tag.id" >{{tag.title}}</g-link>
               &nbsp;&nbsp;
            </span>
            <hr>
          </div>
          <Pager :info="$page.posts.pageInfo"></Pager>
          <!-- Pager -->
          <!-- <div class="clearfix">
            <a class="btn btn-primary float-right" href="#">Older Posts &rarr;</a>
          </div> -->
        </div>
      </div>
    </div>
  </Layout>
</template>

<page-query>
query ($page: Int) {
  allStrapiGeneral {
    edges {
      node {
        id
        title
        subtitle
        cover {
          url
        }
      }
    }
  }
  posts: allStrapiPost(page: $page, perPage: 2) @paginate {
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        id
        title
        created_at
        created_by {
          id
          firstname
          lastname
        }
        tags {
          id
          title
        }
      }
    }
  }
}
</page-query>

<script>
import { Pager } from 'gridsome'
export default {
  name: 'homePage',
  metaInfo: {
    title: 'Hello, world!'
  },
  components: {
    Pager
  },
  computed: {
    general() {
      return this.$page.allStrapiGeneral.edges[0].node
    }
  }
}
</script>

<style>

</style>
