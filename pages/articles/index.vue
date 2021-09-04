<template>
  <div>
    <div class="d-flex justify-content-between align-items-center">
      <h1>Articles</h1>
      <nuxt-link to="/articles/add" class="btn btn-success">Add New</nuxt-link>
    </div>
    <hr />

    <div class="alert alert-success" v-if="$route.params.created == 'yes'">
      Record added successfully
    </div>
    <div class="alert alert-success" v-if="$route.params.deleted == 'yes'">
      Record deleted successfully
    </div>

    <div class="list-group" v-if="articles.length">
      <nuxt-link
        class="list-group-item list-group-item-action"
        :to="'/articles/' + article._id"
        v-for="article in paginate"
        :key="article._id"
      >
        {{ article.title }}
      </nuxt-link>
      <!-- <nuxt-link
        class="list-group-item list-group-item-action"
        :to="'/articles/' + article._id"
        v-for="article in articles"
        :key="article._id"
      >
        {{ article.title }}
      </nuxt-link> -->

    </div>

    <div class="alert alert-info" v-else>No records found.</div>

    <b-pagination
      :total="pagination.total"
      :current.sync="pagination.current"
      :per-page="pagination.perPage"
    >
    </b-pagination>
  </div>
</template>

<script>
export default {
  // async asyncData(context){
  //   const {data} = await context.$axios.get('/api/articles')
  //   return {
  //     articles : data
  //   }
  // },
  data () {
    return {
      articles: [],
      pagination: {
        total: 0,
        current: 1,
        perPage: 2,
        isSimple: false
      }
    }
  },
    mounted () {
      this.$axios.get('/api/articles')
      .then(response => {
        this.articles = response.data
        this.pagination.total = this.articles.length
      })
    },
    computed: {
      paginate () {
        if(this.pagination.current === 1) {
            return this.articles.slice(0, this.pagination.perPage)
          }
          let start = (this.pagination.current - 1) * this.pagination.perPage
          let end = this.pagination.current * this.pagination.perPage
          return this.articles.slice(start, end)
      }
    }
	}
</script>
