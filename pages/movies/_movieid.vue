<template>
    <Loading v-if="$fetchState.pending"/>
    <div v-else class="singleContainer">
        <div class="singleMovie">
        <NuxtLink class="button" :to="{name: 'index'}">Back</NuxtLink>
        <div class="movieInfo">
            <div class="movieImg">
                <img :src="`https://image.tmdb.org/t/p/w500/${movie?.poster_path}`" alt="">
            </div>

            <div class="movieContent">
                <h1>Title: {{ movie.title }}</h1>
                <p class="movie-fact">
                    <span>Tagline:</span> "{{ movie.tagline }}"
                </p>
                <p class="movie-fact">
                    <span>Released:</span> 
            {{ new Date(movie.release_date).toLocaleString('en-us', {
              month: 'long',
              day: 'numeric',
              year: 'numeric'
            }) }}
          </p>
          <p class="movie-fact">
            <span>Duration:</span> {{ movie.runtime }} minutes
          </p>
          <p class="movie-fact">
            <span>Revenue:</span>
             {{ 
            movie.revenue.toLocaleString('en-us', {
                style: 'currency',
                currency: 'USD'
            })
            }}
          </p>
          <p class="movie-fact">
            <span>Overview:</span>{{ movie.overview }}
          </p>
            </div>
        </div>
    </div>
    </div>
</template>
<script>
import axios from 'axios';
import Loading from '../../components/Loading.vue'
export default {
    name: 'single-movie',
    head(){
        return{
            title: this.movie.title
        }
    },
    components: {Loading},
    data(){
        return {
            movie: ''
        }
    },
    async fetch(){
        await this.getSingleMovie()
    },
    fetchDelay: 1000,
    methods: {
        async getSingleMovie(){
            await axios.get(`https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=955f51aa0d4768dacde86b0fe2874f7e&language=en-US`)
            .then(resp => {
                this.movie = resp.data

            }).catch(err => {console.log(err)})
        }
    }
}
</script>
<style lang="scss">
.singleContainer{
    width: 100%;

    .singleMovie{
        width: 80%;
        margin: 0 auto;
        color: #fff;
        min-height: 100vh;
        padding: 48px 0;
        box-sizing: border-box;
        display: flex;
        row-gap: 24px;
        flex-direction: column;
        justify-content: center;

        .movieInfo{
            display: flex;
            align-items: center;
            justify-content: space-between;
            color: #fff;

            .movieImg{
                width: calc(40% - 12px);

                img{
                    max-height: 600px;
                    width: 100%;
                    object-fit: cover;
                }
            }

            .movieContent{
                width: calc(60% - 12px);

                h1{
                    font-size: 4.5rem;
                    font-weight: 500;
                }

                .movie-fact{
                    margin-top: 12px;
                    font-size: 1.25rem;
                    line-height: 150%;

                    span{
                        font-weight: 600;
                        text-decoration: underline;
                    }
                }
                .tagline{
                    font-style: italic;

                    span{
                        font-style: normal;
                    }
                }
            }
        }
}
}

@media screen and (max-width: 899px) {
    .movieInfo{
        flex-direction: column;

        .movieImg{
            width: 100% !important;

            img{
                object-fit: contain !important;
            }
        }

        .movieContent{
            width: 100% !important;
        }
    }
}
</style>