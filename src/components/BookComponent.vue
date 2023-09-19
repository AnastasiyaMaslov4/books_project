<template>
    <li class="book__li">
        <div class="book">
            <div class="book__thumbnail">
                <img v-if="book.volumeInfo.imageLinks" :src="book.volumeInfo.imageLinks.smallThumbnail" alt="book thumbnail">
                <img v-else src="https://divera.kz/img/no-image.jpg" alt="book thumbnail">
            </div>
            <h3>{{ book.volumeInfo.title }}</h3>
            <h5> Author: <span v-for="autor in book.volumeInfo.authors" >{{ autor}} ,</span></h5>
            <p>Category: <span v-if="book.volumeInfo.categories">{{ book.volumeInfo.categories[0] }}</span></p>
        </div>
    </li>
</template>

<script>
    export default {
        name: 'BookComponent',
        props: ['book'],
        data() {
            return {
               
            }
        },

    }
</script>

<style lang="scss" scoped>

    .book__li {
        list-style: none;
    }
    .book {
        background-color: #C5DFE2;
        border-radius: 16px;
        width: 300px;
        padding: 16px;
        height: 400px;
        overflow: hidden;
        &__thumbnail {
            height: 40%;
            & img {
                height: 100%;
            }
        }
    }

</style>