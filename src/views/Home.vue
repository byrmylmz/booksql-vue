<template>
<div>
  <ApolloQuery
    :query="require('../graphql/queries/Categories.gql')"
    :variables="{ name }"
  >
   <template slot-scope="{ result: { data }, isLoading }">
      <!-- Some content -->
      <div v-if="isLoading">Loading...</div>
      <ul v-else>
        <li v-for="category of data.categories" :key="category.id" class="user">
          {{ category.id }}
          {{ category.name}}
        </li>
      </ul>
    </template>
  </ApolloQuery>
</div>
</template>



<script>
// @ is an alias to /src
import gql from 'graphql-tag'

export default {
  name: 'Home',
  components: {
    
  },
  data(){
    return{
    categories:[]
    }
  },

  apollo: {
  categories: gql`{
    categories{
      id
      name
    }
  }`,
},
}
</script>
