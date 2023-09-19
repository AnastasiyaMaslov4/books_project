<template>
  <HeaderComponent/>
  <div class="search">
    <input class="search__input" v-model="searchStr"  @keyup.enter="renderData" type="text" name="searchString" id="searchString" placeholder="Что будем искать">
    <button class="search__btn" @click="renderData">
      <svg height="36px" width="36px" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 512.004 512.004" xml:space="preserve" fill="#000000"><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"></g><g id="SVGRepo_iconCarrier"> <path style="fill:#C5DFE2;" d="M217.6,422.402c-112.922,0-204.8-91.878-204.8-204.8s91.878-204.8,204.8-204.8 s204.8,91.878,204.8,204.8S330.522,422.402,217.6,422.402z"></path> <path style="fill:#6FB0B6;" d="M217.6,256.002c-21.171,0-38.4-17.229-38.4-38.4s17.229-38.4,38.4-38.4s38.4,17.229,38.4,38.4 S238.771,256.002,217.6,256.002z"></path> <path style="fill:#E9EBEF;" d="M217.6,294.402c-68.958,0-122.897-65.954-125.167-68.77l-6.477-8.03l6.477-8.03 c2.27-2.816,56.209-68.77,125.167-68.77s122.897,65.954,125.167,68.77l6.477,8.03l-6.477,8.03 C340.497,228.448,286.558,294.402,217.6,294.402z M217.6,179.202c-21.171,0-38.4,17.229-38.4,38.4s17.229,38.4,38.4,38.4 s38.4-17.229,38.4-38.4S238.771,179.202,217.6,179.202z"></path> <g> <path style="fill:#573A32;" d="M380.254,362.148c-0.06-0.06-0.137-0.077-0.196-0.128c34.193-38.434,55.142-88.917,55.142-144.418 c0-120.175-97.425-217.6-217.6-217.6S0,97.427,0,217.602s97.425,217.6,217.6,217.6c55.501,0,105.984-20.949,144.418-55.151 c0.06,0.06,0.077,0.137,0.128,0.196l128,128c2.5,2.509,5.777,3.755,9.054,3.755c3.277,0,6.554-1.246,9.054-3.746 c5-5.001,5-13.099,0-18.099L380.254,362.148z M217.6,409.602c-105.865,0-192-86.135-192-192s86.135-192,192-192s192,86.135,192,192 S323.465,409.602,217.6,409.602z"></path> <path style="fill:#573A32;" d="M352.734,201.534c-2.423-2.995-60.126-73.532-135.134-73.532S84.89,198.539,82.466,201.534 c-7.552,9.378-7.552,22.75,0,32.128c2.423,3.004,60.126,73.54,135.134,73.54s132.71-70.537,135.134-73.532 C360.286,224.292,360.286,210.912,352.734,201.534z M217.6,281.602c-63.625,0-115.2-64-115.2-64s51.575-64,115.2-64 s115.2,64,115.2,64S281.225,281.602,217.6,281.602z"></path> <path style="fill:#573A32;" d="M217.6,166.402c-28.279,0-51.2,22.921-51.2,51.2s22.921,51.2,51.2,51.2s51.2-22.921,51.2-51.2 S245.879,166.402,217.6,166.402z M217.6,243.202c-14.114,0-25.6-11.486-25.6-25.6c0-14.114,11.486-25.6,25.6-25.6 c14.114,0,25.6,11.486,25.6,25.6C243.2,231.716,231.714,243.202,217.6,243.202z"></path> </g> </g></svg>
    </button>
  </div>
  <div class="parameters">
    <label class="parameters__label" for="category">Category:</label>
    <select v-model="selectCategory" @change="renderData" class="parameters__select" name="" id="category">
      <option value="" selected>all</option>
      <option value="art">art</option>
      <option value="biography">biography</option>
      <option value="computers">computers</option>
      <option value="history">history</option>
      <option value="medical">medical</option>
      <option value="poetry">poetry</option>
    </select>
    <label class="parameters__label" for="order">Order by:</label>
    <select v-model="orderBy" @change="renderData" class="parameters__select" name="" id="order">
      <option value="&orderBy=relevance" selected>relevance</option>
      <option value="&orderBy=newest" >newest</option>
    </select>
  </div>
  <div class="search__result" v-if="data">
  
  <p>Всего по запросу найдено: {{ data.totalItems }} книг.</p>

  <div class="result__div">
    
    <ul class="books__list">
        <BookComponent v-for="book in books" :key="book.id" :book="book"/>
    </ul>

    <div class="books__list__loader" v-if="loading"></div>
    <button class="books__list__btn" @click="loadMore" v-else>Load more</button>

  </div>

  {{ selectCategory }}
    {{ books }}
    <hr>
    {{ data }}
  </div>

  <div class="page__info" v-else>
    <h2>How to use:</h2>
    <p>Type the keyword to find the book. You can also select category and sort results by relevance or newest option.</p>
  </div>

</template>

<script>
import BookComponent from '@/components/BookComponent.vue';
import HeaderComponent from '@/components/HeaderComponent.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    BookComponent,
    HeaderComponent,
  },
  data() {
    return {
      API_key: 'AIzaSyAaNCu37cbq1U8RKepfBA-Vh5gGDRtGfHw',
      searchStr: '',
      data: null,
      books: [],
      renderList: [],
      selectCategory: '',
      orderBy: '&orderBy=relevance',
      startIndex: 0,
      loading: true,
    }
  },
  methods: {
    getData() {
      axios
      .get(`https://www.googleapis.com/books/v1/volumes?q=intitle:${this.searchStr}+subject:${this.selectCategory}${this.orderBy}&startIndex=${this.startIndex}&maxResults=30&key=${this.API_key}`)
      .then(response => (this.data = response.data))
      .then(data => (this.books = data.items))
      .catch(error => console.log(error))
      .finally(() => (this.loading = false));

    },
    renderData() {
      this.loading = true;
      this.getData();
    },
    loadMore() {
      let oldData = this.books;
      this.loading = true;
      this.startIndex += 30;
      axios
      .get(`https://www.googleapis.com/books/v1/volumes?q=intitle:${this.searchStr}+subject:${this.selectCategory}${this.orderBy}&startIndex=${this.startIndex}&maxResults=30&key=${this.API_key}`)
      .then(response => (this.data = response.data))
      .then(data => (this.books = oldData.concat(data.items)))
      .finally(() => (this.loading = false));
    }
  },

}
</script>

<style lang="scss">
@keyframes s3{ 
  100%{transform: rotate(1turn)}
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.search {
  display: flex;
  gap: 16px;
  justify-content: center;
  padding-top: 20px;
  padding-bottom: 20px;
}

.parameters {
  display: flex;
  gap: 16px;
  justify-content: center;
  align-items: center;
  padding-top: 20px;
  padding-bottom: 20px;
  &__label {
    font-size: 20px;
  }
  &__select {
    width: 300px;
    padding: 8px 16px;
    font-size: 20px;
  }
}

.search__input {
  width: 400px;
  padding: 8px 16px;
  font-size: 20px;
}

.books__list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 16px;
  margin-bottom: 32px;
  &__loader {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: 
      radial-gradient(farthest-side,#6FB0B6 94%,#0000) top/8px 8px no-repeat,
      conic-gradient(#0000 30%,#6FB0B6);
    -webkit-mask: radial-gradient(farthest-side,#0000 calc(100% - 8px),#000 0);
    animation:s3 1s infinite linear;
    z-index: 3;
    margin-left: auto;
    margin-right: auto;
  }
  &__btn {
    background-color: #6FB0B6;
    border: none;
    border-radius: 16px;
    padding: 16px 32px;
    color: #fff;
    font-size: 20px;
    transition: all 0.3s;
    &:hover {
      background-color: #C5DFE2;
      color: #6FB0B6;
    }
  }
}
</style>
