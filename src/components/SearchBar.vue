<template>
    <div>
        <form @submit.prevent="busqueda(book,selectedCategory)">
            <label for="Search">Shearch your book</label>
            <input v-if="!filterFlag" disabled id="Search" type="text" placeholder="Search by title...">
            <input v-else id="Search" type="text" placeholder="Search by title..." v-model="book">
            <select name="Category">
                <option value="Choose category" @click="categoryDeselected">Choose category...</option>
                <option v-for="category in array" :key="category.list_name" :value="category"
                        @click="categorySelected(category.list_name_encoded)">
                    {{category.list_name}}
                </option>
            </select>
        </form>
        <Books :result="result"/>
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
            async busqueda(title, cat) {
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

</style>