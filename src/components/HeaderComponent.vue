<template>
    <div class="header__background">
        <p class="header__title">Book search using Google Books API</p>
    </div>
</template>

<script>
    export default {
        name: 'HeaderComponent',
    }
</script>

<style scoped>
    .header__background {
        background: url('https://collegeinfogeek.com/wp-content/uploads/2018/11/Essential-Books.jpg') no-repeat center;
        background-size: cover;
        padding: 100px 50px;
    }

    .header__title {
        color: #fff;
        font-size: 48px;
    }
</style>