<template>
  <div class="home">
    <Hero/>

    <div class="container">
      <div class="search">
      <input type="text" placeholder="Search" v-model.lazy="searchInput" @keyup.enter="$fetch">
      <button class="button" v-show="searchInput !== ''" @click="empty">Clear Search</button>
    </div>
    </div>

    <div class="container movies">
      <!-- Now Streaming Movies -->
      <div id="movie-grid" class="movies-grid" v-if="searchInput === ''">
        <div class="movie" v-for="movie, idx in movies" :key="idx">
        <div class="movie-img">
          <img :src="`https://image.tmdb.org/t/p/w500/${movie?.poster_path}`" alt="">
          <p class="review">{{ movie.vote_average }}</p>
          <p class="overview">{{ movie.overview }}</p>
        </div>

        <div class="info">
          <p class="title">{{ movie.title.slice(0, 35) }} <span v-if="movie.title.length>35">...</span></p>
          <p class="release">
            Released:
            {{ new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric'
            }) }}
          </p>
          <NuxtLink class="button button-light" :to="{name: 'movies-movieid', params: {movieid: movie.id}}">Get More Info</NuxtLink>
        </div>
        </div>
      </div>


      <!-- Loading -->
      <Loading v-if="$fetchState.pending"/>


      <!-- Searched Movies -->
      <div id="movie-grid" class="movies-grid" v-else>
        <div class="movie" v-for="movie, idx in searchedMovies" :key="idx">
        <div class="movie-img">
          <img :src="`https://image.tmdb.org/t/p/w500/${movie?.poster_path}`" alt="">
          <p class="review">{{ movie.vote_average }}</p>
          <p class="overview">{{ movie.overview }}</p>
        </div>

        <div class="info">
          <p class="title">{{ movie.title.slice(0, 35) }} <span v-if="movie.title.length>35">...</span></p>
          <p class="release">
            Released:
            {{ new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric'
            }) }}
          </p>
          <NuxtLink class="button button-light" :to="{name: 'movies-movieid', params: {movieid: movie.id}}">Get More Info</NuxtLink>
        </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Hero from '../components/Hero.vue';
import Loading from '../components/Loading.vue'

export default {
    name: "IndexPage",
    head(){
      return{
        title: "Movie Streaming in Nuxt",
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: 'Get all the latest streaming movies in theaters & online'
          },
          {
            hid: 'keywords',
            name: 'keywords',
            content: 'movies, stream, streaming'
          }
        ]
      }
    },
    components: { Hero, Loading },
    data(){
        return{
            movies: [],
            searchedMovies: [],
            searchInput: ''
        }
    },
    async fetch(){
      if(this.searchInput === ''){
        await this.getMovies()
        return
      } else if (this.searchInput !== '') {
        await this.getSearchedMovies()
      }
    },
    fetchDelay: 1000,
    methods: {
        async getMovies(){
            await axios.get('https://api.themoviedb.org/3/movie/now_playing?api_key=955f51aa0d4768dacde86b0fe2874f7e&language=en-US&page=1')
            .then(resp => {
                const result = resp

                result.data.results.forEach(element => {
                    this.movies.push(element)
                });
                // console.log(this.movies)
            }).catch(err => {console.log(err)})
        },
        async getSearchedMovies(){
          await axios.get(`https://api.themoviedb.org/3/search/movie?api_key=955f51aa0d4768dacde86b0fe2874f7e&language=en-US&page=1&query=${this.searchInput}`)
          .then(resp => {
            resp.data.results.forEach(movie => {
              this.searchedMovies.push(movie)
            })
          }).catch(err => {console.log(err)})
        },
        empty(){
          this.searchInput = ''
          this.searchedMovies = []
        }
    }
}
</script>

<style lang="scss">
.loading{
   padding-top: 120px;
   align-items: flex-start;
}

.container{
  width: 100%;
  margin-top: 48px;
  padding-bottom: 24px;
  box-sizing: border-box;

  .search{
    width: 80%;
    margin: 0 auto;
    display: flex;
    align-items: center;
    column-gap: 12px;

    input{
      max-width: 350px;
      width: 100%;
      padding: 10px 6px;
      box-sizing: border-box;
      font-size: 0.85rem;
      border: none;

      &:focus{
        outline: none;
      }
    }
  }

  .movies-grid{
    width: 80%;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    column-gap: 24px;
    row-gap: 48px;

    .movie{
      width: calc(25% - 24px);
      height: 980px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;

      .movie-img{
        width: 100%;
        height: calc(75% - 24px);
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;

        &:hover{

          p.overview{
            transform: scaleY(1);
          }
        }

        p.review{
          position: absolute;
          top: 0;
          left: 0;
          margin: 0;
          border-radius: 0 0 16px 0;
          padding: 8px 12px;
          box-sizing: border-box;
          background-color: #c92502;
          font-size: 1.25rem;
          line-height: 24px;
          font-weight: 600;
          color: #fff;
        }

        p.overview{
          position: absolute;
          margin: 0;
          bottom: 0;
          padding: 12px 8px;
          box-sizing: border-box;
          transform: scaleY(0);
          transform-origin: bottom;
          background-color: #c92502;
          transition: 0.32s linear;
          font-size: 1rem;
          line-height: 20px;
          font-weight: 400;
          color: #fff;
        }

        img{
          width: 100%;
          height: 100%;
          object-fit: cover;
        }
      }

      .info{
        width: 100%;
        height: 25%;
        display: flex;
        flex-direction: column;
        justify-content: space-between;

        p.title{
          font-size: 1.5rem;
          line-height: 28px;
          font-weight: 700;
          color: #fff;
          margin: 0;
        }

        p.release{
          font-size: 1rem;
          line-height: 20px;
          font-weight: 400;
          color: #fff;
        }
      }
    }
  }
}

@media screen and (max-width: 1440px) and (min-width: 900px) {
  .movies-grid{

    .movie{
      width: calc(50% - 24px) !important;

      .movie-img{
          height: calc(85% - 24px) !important;
        }

        .info{
          height: calc(15% - 12px) !important;
        }
    }
  }
}

@media screen and (max-width: 899px) {
  .movies-grid{

    .movie{
      width: 100% !important;
      height: 780px !important;

        
    }
  }
}
</style>
