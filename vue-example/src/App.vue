<template>
  <div id="app">
    <div>
      <h1>Message Board Wall</h1>
      <ul id="example-1">
        <li v-for="item in posts" :key="item.message">
          {{ item.message }} - <i>From {{item.from}} at {{item.createdAt}}</i>        
        </li>
      </ul>
    </div>
    <div>
      <h2>Submit Message</h2>
      <input type="text" v-model="from" placeholder="From">
      <br/>
      <input type="text" v-model="message" placeholder="Message">
      <br/>
      <button v-on:click="createPost">Create Post</button>
    </div>    
 </div> 
</template>

<script>
import { API } from 'aws-amplify';
import { createWallPost } from './graphql/mutations';
import { listWallPosts } from './graphql/queries';

export default {
  name: 'App',
  async created() {
    this.getWallMessages();
  },
  data() {
    return {
      from: '',
      message: '',
      posts: []
    }
  },
  methods: {
    async createPost() {
      if(!this.from || !this.message) {
        return;
      }
      let p = {
        from: this.from,
        message: this.message
      };      
      await API.graphql({
        query: createWallPost,
        variables: {input: p
        },
      });
      this.from = '';
      this.message = '';   
      this.getWallMessages();   
    },
    async getWallMessages() {
      const posts = await API.graphql(
        {          
          query: listWallPosts
        }
      );
      this.posts = posts.data.listWallPosts.items;      
    }
  }
}
</script>
