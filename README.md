### To Run
- in one terminal tab:
  - `cd backend`
  - `bundle install`
  - `rails db:create db:migrate db:seed`
  - `rails s`
- in a second terminal tab:
  - `cd frontend`
  - `npm install`
  - `npm run serve`
- in browser, go to `http://localhost:8080/`

### To Create From Scratch
- in one terminal tab:
  - `mkdir rails-vue`
  - `cd rails-vue`
  - `rails new backend --api`
  - `cd backend`
  - in `backend/Gemfile` uncomment `gem "rack-cors"` on line 37
  - make `backend/config/initializers/cors.rb` look like this:
  ```
  Rails.application.config.middleware.insert_before 0, Rack::Cors do
    allow do
      origins "*"
      resource "*",
        headers: :any,
        methods: [:get, :post, :put, :patch, :delete, :options, :head]
    end
  end
  ```
  - `rails g scaffold Movie name:string`
  - make `backend/db/seeds.rb` look like this:
  ```
  Movie.create([{ name: "Star Wars" }, { name: "Lord of the Rings" }])
  ```
  - `bundle install`
  - `rails db:migrate db:seed`
  - `rails s`
- in a second terminal tab:
  - `vue create frontend`
    - choose `Vue 2` when it asks you
  - `cd frontend`
  - `npm install --save axios`
  - make `frontend/src/main.js` look like this:
  ```
  import Vue from 'vue'
  import App from './App.vue'
  import axios from 'axios'

  Vue.prototype.$http = axios

  Vue.config.productionTip = false

  new Vue({
    render: h => h(App),
  }).$mount('#app')
  ```
  - make `frontend/src/components/HelloWorld.vue` look like this:
  ```
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
  ```
  - `npm install`
  - `npm run serve`