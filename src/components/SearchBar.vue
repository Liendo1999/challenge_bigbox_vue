<template>
    <div id="container">
        <form @submit.prevent="search(book,selectedCategory)">
            <div class="formGroup">
                <label for="Search">Shearch book</label>
                <input v-if="!filterFlag" disabled id="Search" type="text" placeholder="Search by title...">
                <input v-else id="Search" type="text" placeholder="Search by title..." v-model="book">
            </div>
            <div class="formGroup">
                <label for="Category">Select a category</label>
                <select id="Category" name="Category">
                    <option value="Choose category" @click="categoryDeselected">Choose a category...</option>
                    <option v-for="category in array" :key="category.list_name" :value="category" @click="categorySelected(category.list_name_encoded)">
                        {{category.list_name}}
                    </option>
                </select>
            </div>
        </form>
        <div id="booksContainer">
            <Books id="books" :result="result"/>
        </div>
    </div>
</template>

<script>
    import Books from "@/components/Books";
    import axios from "axios";

    let APIStart = "https://api.nytimes.com/svc/books/v3/lists/current/"
    let APIEnd = ".json?api-key=wMrIxYjKdpTQq76wy7ngPAG1OD0VJy8j"

    export default {
        name: "SearchBar",
        components: {
            Books
        },
        props: {
            array: Array,
        },
        methods: {
            categorySelected(cat) {
                this.filterFlag = true
                this.selectedCategory = cat
            },
            categoryDeselected() {
                this.filterFlag = false
            },
            async search(title, cat) {
                this.result = []
                let API = APIStart + cat + APIEnd
                console.log(API)
                await axios.get(API)
                    .then((response) => this.responseData = response.data.results.books)
                    .catch((error) => {
                        console.log(error)
                    })
                    .then(() => {
                        let titleUp = title.toUpperCase()
                        this.responseData.forEach(book => {
                            if (book.title.includes(titleUp)) {
                                this.result.push(book)
                            }
                        })
                    })
            },
        },
        data() {
            let filterFlag = false;
            let book = "";
            let selectedCategory = "";
            let responseData = {};
            let result = [];
            return {filterFlag, book, selectedCategory, responseData, result}
        }
    }
</script>

<style scoped>
    form{
        display: flex;
        margin: 0 auto;
        width: 80%;
        justify-content: center;
    }
    .formGroup,#container{
        display: flex;
        flex-direction: column;
        margin: 0 2rem;
    }
    input,select{
        width: 500px;
        height: 30px;
        background-color: #403d39;
        border-style: none;
        color: #ccc5b9;
    }
    select{width: 300px;}
    label {
        font-size: 20px;
        margin-bottom: 0.8rem;
    }
    #booksContainer {
        margin-top: 2rem;
        justify-self: center;
    }
</style>