<template>
  <div class="container" :key="status">
    <h1>{{ msg }}</h1>
    
    <div v-if="status=='search'">
      <div class="m-3"> 
        <button type="button" class="btn btn-primary" @click="createNewPost">Create new post</button>
      </div>
      <div class="search-wrapper position-sticky sticky-top">
        <input type="text" class="search-bar form-control" v-model="user_search_input" placeholder="Search by title or body..."/>
      </div>
      <div class="table-wrapper m-3" v-if="postList.length > 0">
        <table v-if="filteredList.length > 0" class="table table-hover">
          <thead>
            <tr class="row" scope="row">
              <th class="col-1" scope="col">ID</th>
              <th class="col-1" scope="col">UserID</th>
              <th class="col-2" scope="col">Title</th>
              <th class="col-7" scope="col">Body</th>
              <th class="col-1" scope="col">Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr class="row" v-for="post in filteredList" :key="post.id">
              <th class="col-1">{{post.id}}</th>
              <td class="col-1">{{post.userId}}</td>
              <td class="col-2">{{post.title}}</td>
              <td class="col-7">{{post.body}}</td>
              <td class="col-1">
                <a class="btn btn-primary" @click="selectPost(post)">Edit</a>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="alert alert-info" v-else>Your search returned no results. Please adjust your search.</div>
      </div>
    </div>

    <div v-if="status=='edit'"> 
      <Post :post="selectedPost" @cancel="backToSearch" @saveNewPost="saveNewPost" @updatePost="updatePost" @deletePost="deletePost"></Post>
    </div>
  </div>
</template>

<script>
import Post from './Post.vue'

export default {
  name: 'Arvis',
  props: {
    msg: String
  },
  components:{
    Post
  },
  data() {
    return {
      user_search_input: '',
      postList: [],
      status: 'search',
      selectedPost: ''
    }
  },
  computed: {
    filteredList() {
      return this.postList.filter(post => {
        return (post.title+' '+post.body).toLowerCase().replace(/\r?\n|\r/g, ' ').includes(this.user_search_input.toLowerCase())
      })
    }
  },
  methods: {
    selectPost(post) {
      this.selectedPost = post
      this.status = 'edit'
    },

    createNewPost() {
      this.selectedPost = {id: '',
                          userId: 1,
                          title: '',
                          body: ''}
      this.status = 'edit'
    },

    saveNewPost(post) {
      post.id = this.postList[this.postList.length-1].id + 1
      this.postList.push(post)
      this.backToSearch()
    },

    updatePost(post) {
      this.postList[this.postList.indexOf(this.selectedPost)] = post
      this.selectedPost = post
      this.backToSearch()
    },

    deletePost() {
      this.postList.splice(this.postList.indexOf(this.selectedPost), 1)
      this.backToSearch()
    },

    backToSearch() {
      this.status = 'search'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(response => response.json())
      .then(json_response => (this.postList = json_response))
  }
}
</script>

<style scope>
  .search-bar {
    box-shadow: 0px 4px 8px rgb(0 0 0 / 20%)
  }
</style>