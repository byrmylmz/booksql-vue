<template>
  <div class="book">
       <!-- APOLLO COMPONENT -->
      <ApolloQuery :query="require('../graphql/queries/Book.gql')" :variables="{ id: $route.params.id }" >
        <template slot-scope="{ result: { data }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <ul v-else>
              <div>{{data.book.title}}</div>
              <div>{{data.book.category.name}}</div>
            <div>{{data.book.author}}</div>
            <img :src="`http://booksql-laravel.test/img/${data.book.image}`" alt="">
            <div>
                <router-link :to="`/books/${data.book.id}/edit`" class="link-margin">Edit</router-link>
                <!-- <a href="#" class="link-margin" @click.prevent="deleteBook">Delete</a> -->
            </div>
          </ul>
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