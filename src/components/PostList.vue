<script>
// Родитель: App
import PostItem from './PostItem.vue'
export default {
    props: {
        posts: {
            type: Array,
            requered: true
        }
    },
    // methods: {
    //     removePost(){
    //         this.$emit('remove', post)
    //     }
    // },
    components: { PostItem }
}
</script>

<template>
    <div v-if="posts.length > 0">
        <h3>Список элементов</h3>
        <transition-group name="post-list">
            <post-item v-for="post in posts" :post="post" :key="post.id" @remove="$emit('remove', post)" />
        </transition-group>
    </div>
    <h2 v-else>Список пуст</h2>
</template>

<style scoped>
.post-list-item {
  display: inline-block;
  margin-right: 10px;
}
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.4s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}
.post-list-move {
  transition: transform 0.4s ease;
}
</style>