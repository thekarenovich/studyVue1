<script>
// Родитель: PostList
import MyButton from './UI/MyButton.vue'
export default {
    props: {
        post: {
            type: Object,
            required: true
        }
    },
    // methods: {
    //     removePost(){
    //         this.$emit('remove', post)
    //     }
    // },
    components: { MyButton }
}
</script>

<template>
    <div class="post">
        <div>
            <div><strong>Название:</strong> {{ post.title }}</div>
            <div><strong>Описание:</strong> {{ post.body }}</div>
        </div>
        <div class="post__btns">
            <my-button v-on:click="$emit('remove', post)">Удалить</my-button>
        </div>
    </div>    

</template>

<style scoped>
.post {
    padding: 15px;
    border: 2px solid teal;
    margin-top: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;
}
</style>