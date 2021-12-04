<template>
  <div class="create container mt-12">
    <h1 class="mb-4">Create Category</h1>
    <form action="#" method="POST" @submit.prevent="createCategory">
      
      <div class="form-group">
        <label class="font-bold mb-2" for="title">Title</label>
        <input type="text" name="name" id="name" v-model="name">
      </div>
 
      <div class="form-group">
        <button type="submit">Add Category</button>
      </div>

    </form>
  </div>
</template>

<script>
import CATEGORY_ALL from '@/graphql/queries/Categories.gql'
import CATEGORY_CREATE from '@/graphql/mutations/AddCategory.gql'

export default {
        data(){
            return{
                
                name:''
            }
        },

methods: {
  createCategory() {
    // We save the user input in case of an error
    const name = this.name
    // We clear it early to give the UI a snappy feel
    this.name = ''
    // Call to the graphql mutation
    this.$apollo.mutate({
      // Query
      mutation:CATEGORY_CREATE,
      // Parameters
      variables: {
        name:name,
      },
      // Update the cache with the result
      // The query will be updated with the optimistic response
      // and then with the real result of the mutation
      update: (store, { data: { createCategory } }) => {
        // Read the data from our cache for this query.
        const { data } = store.readQuery({ query: CATEGORY_ALL })
        // Add our tag from the mutation to the end
        // We don't want to modify the object returned by readQuery directly:
        // https://www.apollographql.com/docs/react/caching/cache-interaction/
        // const namesCopy = names.slice()
        // namesCopy.push(createCategory)
        data.categories.push(createCategory)
        // Write our data back to the cache.
        
        store.writeQuery({ query: CATEGORY_ALL, data})
        /**
         * SECOND PART
         */
        // const categories  = {query:CATEGORY_ALL}
        // const categoriesData=store.readQuery(categories)
        // categoriesData.categories.push(createCategory)
        // store.writeQuery({...categories,data: categoriesData})
      },
      // Optimistic UI
      // Will be treated as a 'fake' result as soon as the request is made
      // so that the UI can react quickly and the user be happy
        optimisticResponse: {
        __typename: 'Mutation',
        createCategory: {
          __typename: 'Category',
          id: null,
          name: name,
        },
      },
    }).then((data) => {
      // Result
      console.log(data)
       this.$router.push('/')
    }).catch((error) => {
      // Error
      console.error(error)
      // We restore the initial user input
      this.name = name
      })
    },
  },
}
</script>

<style scoped>
  .form-group {
    margin-bottom: 32px;
  }
  input[type="text"], textarea {
    padding: 10px 14px;
    border: 1px solid lightgray;
    border-radius: 5px;
  }
  label {
    display: block;
  }
  button {
    padding: 16px;
    background: #027BFF;
    color: white;
    border-radius: 5px;
    font-size: 16px;
  }
</style>