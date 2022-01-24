<template>
  <div class="container col-md-8">
    <label>Post title:</label>
    <input type="text" class="form-control m-2" v-model="selectedPost.title" placeholder="Title"/>
    <label>Post body:</label>
    <textarea class="form-control m-2" rows="6" v-model="selectedPost.body" placeholder="Body"/>

    <div v-if="errors.length!==0" class="form-control alert alert-danger float-left m-2" align="left" role="alert">
      <span>Please correct following errors:</span>
      <ul>
        <li v-for="error in errors" :key="error">{{error}}</li>
      </ul>
    </div>
    <button v-if="selectedPost.id !== ''" type="button" class="btn btn-danger m-2 float-left" @click="deletePost">Delete post</button>
    <button type="button" class="btn btn-secondary m-2 float-right" @click="cancel">Cancel</button>
    <button type="button" class="btn btn-success m-2 float-right" @click="updatePost">Save</button>
  </div>
</template>

<script>
export default {
  name: 'Post',
  props: {
    post: Object
  },
  data() {
    return { 
      errors: [],
      selectedPost: {
        id: this.post.id,
        userId: this.post.userId,
        title: this.post.title,
        body: this.post.body
      }
    }
  },
  methods: {
    cancel() {
      this.$emit('cancel')
    },
    
    deletePost() {
      if(confirm('Are you sure?'))
        this.$emit('deletePost')
    },

    updatePost() {
      this.errors = []
      if (this.selectedPost.title.length == 0) {
        this.errors.push('Title is required')
      }
      if (this.selectedPost.body.length == 0) {
        this.errors.push('Body is required')
      }
      if (this.errors.length !== 0) {
        return 
      }

      if (this.post.id=='')
        this.$emit('saveNewPost', this.selectedPost)
      else this.$emit('updatePost', this.selectedPost)
    }
  }
}
</script>
