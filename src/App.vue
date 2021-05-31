<template>
    <div id="app">
        <header>
            <h1>BigBox Challenge - Julian Liendo</h1>
            <h4>NYTimes Books API</h4>
        </header>
        <SearchBar :array="arrayLists"/>
        <Books/>
    </div>
</template>

<script>
    import SearchBar from "@/components/SearchBar";
    import Books from "@/components/Books";
    import axios from "axios";

    const API = "https://api.nytimes.com/svc/books/v3/lists/names.json?api-key=wMrIxYjKdpTQq76wy7ngPAG1OD0VJy8j"
    export default {
        name: 'App',
        components: {
            SearchBar,
            Books
        },
        data() {
            return {
                responseData: {},
                arrayLists: [],
            }
        },
        methods: {
            async fetchApi() {
                await axios.get(API)
                    .then((response) => this.responseData = response.data.results)
                    .catch((error) => {
                        console.log(error)
                    })
            },
            cut() {
                this.arrayLists = this.responseData.slice(0, 10)
            },
        },
        mounted() {
            this.fetchApi().then(this.cut)

        }
    }
</script>

<style>
    body {
        background-color: #252422;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    h1 {color: #eb5e28;font-size: 36px}
    h4 {font-size: 30px}
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        margin-top: 60px;
        color:#ccc5b9;
    }
</style>
