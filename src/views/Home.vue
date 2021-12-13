<template>
  <div class="home">
    <!-- HERO SECTION STARTED -->
    <div class="hero bg-gray-100 mb-16 ">
      <div class="container mx-auto flex flex-col lg:flex-row lg:justify-between py-10 px-40">
        <div class="mt-10">
          <h1 class="w-full text-2xl font-semibold lg:w-3/4 mb-4">Book recommendation site built with GraphQL</h1>
          <p class="leading-normal w-full lg:w-3/4 mb-6">Built with Laravel (Lighthouse GraphQL), Vue.js (vue-apollo) and Tailwind CSS</p>
          <div class="flex items-center">
            <a href="#" class="bg-purple-900 text-white rounded px-4 py-4 mr-4 hover:bg-purple-900">View Books</a>
            <a href="#" class="border border-purple-900 border-solid rounded text-purple-900 px-4 py-4 hover:bg-purple-900 hover:text-white">View Screencasts</a>
          </div>
        </div>
        <div class="mt-10 lg:mt-0">
          <img src="../assets/hero.svg" alt="hero">
        </div>
      </div>
    </div> <!-- end hero -->
    
    <!-- CONTAINER FOR 2 COLUMN
     -->
    <div class="container mx-auto px-40">
    <div class="flex flex-wrap -mx-4">
    <div class="w-full lg:w-1/4 px-4 mb-12 bg-blue-50 border border-dashed  border-gray-500">
    <!-- CATEGORIES APOLLO COMPONENT LEFT SIDE -->
        <ApolloQuery :query="categoriesQuery">
      <template slot-scope="{ result: { data }, isLoading }">
        <!-- Some content -->
        <div v-if="isLoading">Loading...</div>
        <ul v-else class="list-reset text-lg mt-5"> 
           <!-- ADD A BOOK -->
          <li class="mb-5">
             <router-link to="/books/add"><button class="flex items-center bg-gray-300 px-2 border rounded-md hover:bg-gray-200">New book</button> </router-link>
          </li>
          <!-- LIST FOR ALL BOOKS -->
          <li class="mb-5">
            <a href="#" class="link-margin" @click.prevent="selectCategory('all')" >All</a>
          </li>
          <!-- LIST FOR FEATURED BOOKS -->
          <li class="mb-5">
            <a href="#" class="link-margin" @click.prevent="selectCategory('featured')">Featured</a>
          </li>
          <!-- LINK FOR THE LIST -->
            <a v-for="category of data.categories" :key="category.id" >
          <li class="mb-5">
                <a href="#" class="link-margin" @click.prevent="selectCategory(category.id)">{{ category.name }}</a>
          </li>
            </a>
           
        </ul>
      </template>
    </ApolloQuery> 
    <!-- END CATEGORY COMPONENT -->
     </div>
        <div class="w-full lg:w-3/4 px-4 mb-12 border border-dashed border-gray-500 bg-blue-50">
          <div>
            <!-- BOOKS LIST COMPONENT RIGHT SIDE -->
                <!-- SECOND COMPONENT ALL BOOKS-->
                  <div v-if="selectedCategory === 'all'">
                    <ApolloQuery :query="query">
                      <template slot-scope="{ result: { data }, isLoading }">
                        <div v-if="isLoading">Loading...</div>
                        <ul v-else class="flex flex-wrap">
                          <div v-for="book of data.books" :key="book.id" class="w-full lg:w-1/3 px-4 mb-12">
                          <book-listing :book="book"></book-listing>      
                          </div>
                        </ul>
                      </template>
                    </ApolloQuery>
                  </div>

                  <!-- THIRD COMPONENT FEATURED BOOKS-->
                  <ApolloQuery :query="query" :variables="{ featured: true}" v-else-if="selectedCategory === 'featured'" >
                    <template slot-scope="{ result: { data }, isLoading }">
                      <div v-if="isLoading">Loading...</div>
                      <ul v-else class="flex flex-wrap">
                        <div v-for="book of data.booksByFeatured" :key="book.id" class="w-full lg:w-1/3 px-4 mb-12" >
                          <book-listing :book="book"></book-listing>               
                        </div>
                      </ul>
                    </template>
                  </ApolloQuery>

                  <!-- FORTH COMPONENT  NORMAL BOOKS BY ID-->
                  <div :query="query" :variables="{ id: selectedCategory }" v-else>
                    <ApolloQuery :query="query" :variables="{ id: selectedCategory }">
                      <template slot-scope="{ result: { data }, isLoading }">
                        <div v-if="isLoading">Loading...</div>
                        <ul v-else class="flex flex-wrap">
                          <div v-for="book of data.category.books" :key="book.id" class="w-full lg:w-1/3 px-4 mb-12" >
                            <book-listing :book="book"></book-listing>    
                          </div>
                        </ul>
                      </template>
                    </ApolloQuery>
                  </div>
                  <!-- END  -->
         </div>
        </div>

      </div>
    </div> <!-- end container -->





  </div>
</template>

<script>
// @ is an alias to /src
import categoryQuery from '@/graphql/queries/Category.gql'
import categoriesQuery from '@/graphql/queries/Categories.gql'
import booksQuery from '@/graphql/queries/Books.gql'
import booksFeaturedQuery from '@/graphql/queries/BooksFeatured.gql'
import bookListing from '@/components/BookListing.vue'

export default {
   name: 'home',
  components: {
    bookListing
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