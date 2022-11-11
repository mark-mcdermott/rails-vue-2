<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul>
      <li v-for="movie in movies" v-bind:key="movie.name">
        {{ movie.name }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      movies: []
    }
  },
  mounted() {
    try {
      const baseURI = 'http://localhost:3000/movies'
      this.$http.get(baseURI)
      .then((result) => {
        this.movies = result.data
        console.log(this.movies)
      })
    } catch (error) {
      console.log(error)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
