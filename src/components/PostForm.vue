<script>
// Родитель: App
import MyButton from './UI/MyButton.vue'
import MyInput from './UI/MyInput.vue'
export default {
    data() {
        return {
            post: {
                title: '',
                body: ''
            }
        }
    },
    methods: {
        // Для title и body в input'ах:
        // inputTitle(event) { this.post.title = event.target.value; },
        // inputBody(event) { this.post.body = event.target.value; }

        createPost() {
            this.post.id = Date.now()
            this.$emit('create', this.post)
            this.post = {
                title: '',
                body: ''
            }
        }
    },
    components: { MyButton, MyInput }
}
</script>

<template>
    <form @submit.prevent>
        <h4>Создание поста</h4>
        <!-- v-model="post.title" вместо v-bind:value="post.title" v-on:input="inputTitle($event)" -->
        <!-- Связали с моделью title тег input  v-bind:value="title" -->
        <!-- Отслеживаем изменения в теге input  v-on:input="inputTitle($event) -->
        <my-input v-model="post.title" style="width: 90%;" type="text" placeholder="Название"/>
        <my-input v-model="post.body" style="width: 90%;" type="text" placeholder="Описание"/>
        <my-button style="align-self: flex-end; margin-top: 15px;" v-on:click="createPost">Создать</my-button>
    </form>
</template>

<style scoped>
form {
    display: flex;
    flex-direction: column;
}
</style>