<template>
  <div class="home">
    <router-link to="/books/add">Add a book</router-link>
    <!-- FIRST COMPONENT -->
    <ApolloQuery :query="categoriesQuery">
      <template slot-scope="{ result: { data }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading">Loading...</div>
        <ul v-else>
          <a href="#" class="link-margin" @click.prevent="selectCategory('all')" >All</a>
           <a href="#" class="link-margin" @click.prevent="selectCategory('featured')">Featured</a>
           <a v-for="category of data.categories" :key="category.id" >
              <a href="#" class="link-margin" @click.prevent="selectCategory(category.id)">{{ category.name }}</a>
          </a>
        
        </ul>
      </template>
    </ApolloQuery>

    <!-- SECOND COMPONENT -->
    <div v-if="selectedCategory === 'all'">
      <ApolloQuery :query="query">
        <template slot-scope="{ result: { data }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <ul v-else>
           
            <div v-for="book of data.books" :key="book.id" class="link-margin">
             <router-link :to="`/books/${book.id}`">
              {{ book.id }}.{{ book.title }}
             </router-link> 
             <!-- image cover -->
             <div>{{book.author}}</div>
             <img :src="`http://booksql-laravel.test/img/${book.image}`" alt="">
            </div>
            
          </ul>
        </template>
      </ApolloQuery>
    </div>

    <!-- THIRD COMPONENT -->
    <ApolloQuery
      :query="query"
      :variables="{ featured: true}"
      v-else-if="selectedCategory === 'featured'"
    >
      <template slot-scope="{ result: { data }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <ul v-else>
          <div
            v-for="book of data.booksByFeatured"
            :key="book.id"
            class="link-margin"
          >
             <router-link :to="`/books/${book.id}`">
              {{ book.id }}.{{ book.title }}
             </router-link> 
             <!-- image cover -->
              <div>{{book.author}}</div>
             <img :src="`http://booksql-laravel.test/img/${book.image}`" alt="">
             
             
          </div>
          
        </ul>
      </template>
    </ApolloQuery>

    <!-- FORTH COMPONENT -->
    <div :query="query" :variables="{ id: selectedCategory }" v-else>
      <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
        <template slot-scope="{ result: { data }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <ul v-else>
            <div
              v-for="book of data.category.books"
              :key="book.id"
              class="link-margin"
            >
             <router-link :to="`/books/${book.id}`">
              {{ book.id }}.{{ book.title }}
             </router-link> 
             <!-- image cover -->
             <div>{{book.author}}</div>
             <img :src="`http://booksql-laravel.test/img/${book.image}`" alt="">
            </div>
          </ul>
        </template>
      </ApolloQuery>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import categoryQuery from '@/graphql/queries/Category.gql'
import categoriesQuery from '@/graphql/queries/Categories.gql'
import booksQuery from '@/graphql/queries/Books.gql'
import booksFeaturedQuery from '@/graphql/queries/BooksFeatured.gql'

export default {
  name: 'home',
  components: {
    
  },
  data() {
    return {
      categoryQuery,
      categoriesQuery,
      booksQuery,
      booksFeaturedQuery,
      selectedCategory: 'all',
      query: booksQuery,
      categories: []
    }
  },
  methods: {
    selectCategory(category) {
      if (category === 'all') {
        this.query = booksQuery
      } else if (category === 'featured') {
        this.query = booksFeaturedQuery
      } else {
        this.query = categoryQuery
      }
      this.selectedCategory = category
    }
  }
}
</script>

<style>
.link-margin {
  margin-right: 24px;
}
</style>
