//TEMPLATE
<template>
<div class='tag'>
  <!-- <h1 class=''>#{{ props.tag }}</h1> -->
  <div v-if="error">{{ error }}</div>
  <div v-if="posts.length" class="layout">
    <div>
      <h1 class=''>Posts with #{{ props.tag }}</h1>
      <PostList :posts="tagPosts" />
    </div>
    <TagCloud :posts="tagPosts" />
  </div>
  <div v-else>
    <Spinner />
  </div>
</div>
</template>


//SCRIPT
<script setup>
import PostList from '../components/PostList.vue'
import Spinner from '../components/Spinner.vue'
import TagCloud from '../components/TagCloud.vue'
import getPosts from '../composables/getPosts'
import { computed } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const props = defineProps(['tag'])
const { posts, error, load } = getPosts()
load()
console.log('route: ', route)

const tagPosts = computed(() => {
  // return posts.value.filter(p => p.tags.includes(props.tag))
  return posts.value.filter(p => p.tags.includes(route.params.tag))
})

</script>


//STYLE
<style>
.layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}
.tag {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
</style>