<template>
  <div v-if="posts && posts.length">
    <div v-for="post of posts">
      <a :href="post.url"><strong>[{{post.score}}] {{post.title}}</strong></a><br><br>
    </div>
  </div>

  <ul v-if="errors && errors.length">
    <li v-for="error of errors">
      {{error.message}}
    </li>
  </ul>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      posts: [],
      errors: []
    }
  },
  async created() {
    try {
      const response = await axios.get(`https://hacker-news.firebaseio.com/v0/beststories.json`)
      for(var i=0 ; i<response.data.length; i++){
        const result = await axios.get(`https://hacker-news.firebaseio.com/v0/item/`+response.data[i]+`.json`)
        this.posts.push(result.data)
      }
    } catch (e) {
      this.errors.push(e)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
