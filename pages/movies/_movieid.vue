<template>
    <div>
        <loading v-if="$fetchState.pending" />

       <div class="container single-movie">
           <nuxt-link class="button" to="/">Back to Home</nuxt-link>
           <div class="movie-info">
               <div class="movie-img">
                     <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt=""/>
               </div>
               <div class="movie-content">
                   <h1>Title: {{movie.title}}</h1>
                   <p class="movie-fact tagline">
                       <span>TagLine:</span> {{movie.tagline}}
                   </p>
                   <p class="movie-fact">
                       <span> Released:</span>
                 {{
                   new Date(movie.release_date).toLocaleString('en-us',{
                     month:'long',
                     day: 'numeric',
                     year: 'numeric'
                   })
                 }}
                   </p>
                   <p class="movie-fact">
                       <span>Duration:</span> {{movie.runtime}} minute
                   </p>
                   <p class="movie-fact">
                       {{movie.overview}}
                   </p>
               </div>
           </div>
       </div>
    </div>
</template>

<script>
import axios from 'axios'
import Loading from '../../components/Loading.vue'
    export default {
  components: { Loading },
        name:"single-movie",
        data() {
            return {
                movie: ""
            }
        },
        async fetch(){
            await this.getSinleMovie()
        },
        fetcgDelay:1000,
        methods:{
            async getSinleMovie(){
                const data = axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=${process.env.API}&language=en-US`)
                const res = await data
                this.movie = res.data
            }
        }
    }
</script>

<style lang="scss" scoped>
.single-movie {
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 32px 16px;
  .button {
    align-self: flex-start;
    margin-bottom: 32px;
  }
  .movie-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 32px;
    color: #fff;
    @media (min-width: 800px) {
      flex-direction: row;
      align-items: flex-start;
    }
    .movie-img {
      img {
        max-height: 500px;
        width: 100%;
        @media (min-width: 800px) {
          max-height: 700px;
          width: initial;
        }
      }
    }
    .movie-content {
      h1 {
        font-size: 56px;
        font-weight: 400;
      }
      .movie-fact {
        margin-top: 12px;
        font-size: 20px;
        line-height: 1.5;
        span {
          font-weight: 600;
          text-decoration: underline;
        }
      }
      .tagline {
        font-style: italic;
        span {
          font-style: normal;
        }
      }
    }
  }
}
</style>