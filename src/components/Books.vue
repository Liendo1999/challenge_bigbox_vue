<template>
    <div v-if="result.length > 0" class="resultContainer">
        <div class="bookContainer" v-for="book in result" :key="book.title"
             v-bind="templateGenerator(book.title,book.author)">
            <a target="_blank" :href="googleSearch">
                <img :src="book.book_image" :title="book.title" alt="book image" height="300px">
            </a>
            <div class="bookInfo">
                <p><b>Title:</b> {{book.title}}</p>
                <p><b>Description:</b><i> {{book.description}}</i></p>
                <p><b>Author:</b> {{book.author}}</p>
                <a target="_blank" :href="googleSearch">Google search</a>
            </div>
        </div>
    </div>
    <div v-else>Waiting for your search</div>
</template>

<script>
    export default {
        name: "Books",
        props: {
            result: Array,
        },
        data() {
            let googleTemplate = "https://www.google.com/search?q="
            let googleSearch
            return {googleTemplate, googleSearch}
        },
        methods: {
            templateGenerator(title, author) {
                let lowerT = title.toLowerCase()
                let lowerA = author.toLowerCase()
                lowerT = lowerT.replace(/ /g, "+")
                lowerA = lowerA.replace(/ /g, "+")
                this.googleSearch = this.googleTemplate + lowerT.concat("+", lowerA)
            }
        },

    }
</script>

<style scoped>
    .resultContainer {
        background-color: #ccc5b9;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 2rem;
        width: 80%;
        margin: 0 auto;
    }

    .bookContainer {
        width: 60%;
        background-color: rgb(154, 145, 131);
        color: #252422;
        display: flex;
        justify-content: center;
        padding: 1rem;
        margin-bottom: 1rem;
    }

    .bookInfo {
        width: 60%;
        text-align: left;
        margin-left: 1rem;
        font-size: 20px;
    }

    a {
        color: #bd3e0b;
    }
</style>