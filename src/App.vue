<script>
import PostList from './components/PostList.vue'
import PostForm from './components/PostForm.vue'
import MyDialog from './components/UI/MyDialog.vue'
import MyButton from './components/UI/MyButton.vue'
import MySelect from './components/UI/MySelect.vue'
import MyInput from './components/UI/MyInput.vue'
import axios from 'axios'
export default {
    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostsLoading: false,
            selectedSort: '',
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPages: 0,
            sortOptions: [
                { value: 'title', name: 'По названию' },
                { value: 'bidy', name: 'По содержанию' }
            ]
        }
    },
    methods: {
        async fetchPosts() {
            try {
                this.isPostsLoading = true
                setTimeout(async () => {
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _page: this.page,
                            _limit: this.limit
                        }
                    })
                    this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                    this.posts = response.data
                    this.isPostsLoading = false
                }, 1000)
            } catch (e) {
                alert('Ошибка')
            }
        },
        changePage(pageNumber){
            this.page = pageNumber
        },
        createPost(post) {
            this.posts.push(post)
            this.dialogVisible = false
        },
        removePost(post) { this.posts = this.posts.filter(p => p.id !== post.id) },
        showDialog() { this.dialogVisible = true }
    },
    mounted() { this.fetchPosts() },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    },
    watch: {
        page() {
            this.fetchPosts()
        }
    },
    components: { PostList, PostForm, MyDialog, MyButton, MySelect, MyInput }
}
</script>

<template>
    <div class="app">
        <h1>Страница с постами</h1>
        <my-input v-model="searchQuery" style="width: 100%;" placeholder="Поиск..." />
        <div class="app__btns">
            <my-button v-on:click="showDialog">Создать пост</my-button>
            <my-select v-model="selectedSort" :options="sortOptions" />
        </div>
        <my-dialog v-model:show="dialogVisible"><post-form v-on:create="createPost" /></my-dialog>
        <!-- Связали модель posts из PostList и значение posts из App с тегорм post-list -->
        <post-list v-bind:posts="sortedAndSearchedPosts" v-on:remove="removePost" v-if="!isPostsLoading" />
        <div v-else>Идет загрузка...</div>
        <div class="page__wrapper">
            <div v-for="pageNumber in totalPages" :key="pageNumber" class="page" :class="{
                'current-page': page === pageNumber
            }" @click="changePage(pageNumber)">{{ pageNumber }}</div>
        </div>
    </div>
</template>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.app {
    padding: 20px;
}

.app__btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;
}

.page__wrapper {
    display: flex;
    margin-top: 15px;
}

.page {
    border: 1px solid black;
    padding: 10px;
}

.current-page {
    border: 2px solid teal;
}
</style>