<template>
  <Layout>
  <div>
      <h1>Message Board Wall</h1>
      <ul id="example-1">
        <li v-for="item in posts" :key="item.message">
          {{ item.message }} - <i>From {{item.from}} at {{item.createdAt}}</i>
        </li>
      </ul>
    </div>

  </Layout>
</template>

<script>
import { API } from 'aws-amplify';
import { listWallPosts } from '../graphql/queries';

export default {
  data () {
    return {
      posts: []
    }
  },
  async mounted () {
    const posts = await API.graphql(
        {
          query: listWallPosts
        }
    );
    this.posts = posts.data.listWallPosts.items;
    console.log(this.posts);
    
  }
}
</script>

<style>
.home-links a {
  margin-right: 1rem;
}
</style>