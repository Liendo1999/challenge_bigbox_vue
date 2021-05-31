<template>
    <div class="resultContainer">
        <div v-for="book in result" :key="book.title" v-bind="templateGenerator(book.title,book.author)">
            <img :src="book.book_image" alt="book image">
            title: {{book.title}}
            description: {{book.description}}
            author: {{book.author}}
            <a target="_blank" :href="googleSearch">Google search</a>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Books",
        props:{
            result: Array,
        },
        data(){
            let googleTemplate = "https://www.google.com/search?q="
            let googleSearch
            return{googleTemplate,googleSearch}
        },
        methods:{
            templateGenerator(title,author){
                let lowerT = title.toLowerCase()
                let lowerA = author.toLowerCase()
                lowerT = lowerT.replace(/ /g,"+")
                lowerA = lowerA.replace(/ /g,"+")
                this.googleSearch = this.googleTemplate+lowerT.concat("+",lowerA)
            }
        },

    }
</script>

<style scoped>

</style>