<template>
  <div class="book container mx-auto px-40">
    <ApolloQuery :query="require('@/graphql/queries/Book.gql')" :variables="{ id: $route.params.id}">
      <template slot-scope="{ result: { data }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else class="flex mt-16 flex-col lg:flex-row">
          <div>
            <img :src="`http://booksql-laravel.test/img/${data.book.image}`" alt="book cover">
          </div>

          <div class="w-full lg:w-2/3 ml-0 mt-8 lg:mt-0 lg:ml-16">
            <div class="text-4xl font-bold">{{ data.book.title }}</div>
            <div class="text-2xl text-grey-darkest mb-8">{{ data.book.author }}</div>
            <div class="text-lg text-grey-darkest leading-normal">{{ data.book.description }}</div>
            <div class="my-12">
              <a :href="data.book.link" target="_blank" class="border border-purple-900 border-solid rounded text-purple-900 px-4 py-4 hover:bg-purple-900 hover:text-white">View Link</a>
            </div>
            <router-link :to="`${data.book.id}/edit`" href="#" class="">Edit</router-link>
            &middot;
            <a href="#" class="" @click.prevent="deleteBook">Delete</a>

          </div>
          <div>
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>

import deleteBook from '@/graphql/mutations/DeleteBook.gql'
export default {
   
  
  methods: {
    deleteBook() {
      this.$apollo.mutate({
        mutation: deleteBook,
        variables: {
          id: this.$route.params.id,
        }
      }).then(() => {

        this.$router.push('/')
      })
    }
  }
}
</script>