<template>  
  <div class="home">
    <section>
      <slider-movie-items :slides="nowPlayingMovieItems" :width="slideWidth"></slider-movie-items>
    </section>
    <div class="buster-light">
      <div class="movie-items">
        <div class="container">
          <div class="row ipad-width">
            <section>
              <popular-movies :slides="popularMovieItems" :width="slideWidth"></popular-movies>
            </section>    
            <section>
              <recommended-movies v-show="isLoginUser" :slides="recommendedMovieItems" :width="slideWidth"></recommended-movies>
            </section>
            <section>
              <playlist-movies :slides="playlistMovieItems" :width="slideWidth"></playlist-movies>
            </section>
          </div>
        </div>
      </div>
    </div>    
  </div>  
</template>

<script>
import axios from 'axios'
// import axios from 'axios'
import PlaylistMovies from '../components/Home/PlaylistMovies.vue'
import popularMovies from '../components/Home/PopularMovies.vue'
import RecommendedMovies from '../components/Home/RecommendedMovies.vue'
// import SliderMovieItems from '../components/Home/SliderMovieItems.vue'
// @ is an alias to /src
const API = process.env.VUE_APP_BACKEND_URL
// const api = 'http://127.0.0.1:8000/api/v1/movies/mainmovies/'

const SliderMovieItems = () => import('../components/Home/SliderMovieItems.vue');


export default {
  name: 'Home',
  components: {
    SliderMovieItems,
    popularMovies,
    RecommendedMovies,
    PlaylistMovies,
  },
  props: {
    isLoading: Boolean,
    toggleLoading: Function
  },
  data () {
    return {      
      isLoginUser: false,
      nowPlayingMovieItems: [],
      popularMovieItems: [],
      recommendedMovieItems: [],
      playlistMovieItems: [],
      slideWidth: null,
    }
  },
  methods: {
    getMovieItems () {
      // this.toggleLoading(true)
      axios({
        method: 'get',
        url: `${API}/api/v1/movies/mainmovies/` 
      })
        .then(res => {
          // console.log(res.data)        
          this.nowPlayingMovieItems = res.data['now-playing']
          this.popularMovieItems = res.data['popular']
          this.playlistMovieItems = res.data['high-rates']
          // this.toggleLoading(false)
        })
        .catch(err => {
          console.log(err)
        })
    },
    getRecommended () {
      axios({
          method: 'get',
          url: `${API}/api/v1/movies/recommended/`,          
          headers: {Authorization: `JWT ${localStorage.getItem('jwt')}`}
        })
        .then(res => {
          // console.log(res.data)
          this.recommendedMovieItems = res.data
        })
        .catch(err => {
          console.log(err)
        })
    },
    handdler () {
			if (425 < window.innerWidth) {
				this.slideWidth = null
				console.log('425 < window.innerwidth')
			}	else if (window.innerWidth <= 375 ) {
				console.log('window.innerwidth <= 375')
				this.slideWidth = 300
			} else {
				this.slideWidth = 350
				console.log('375 < window.innerwidth <= 425')
			}
		},
    // toggleLoading () {

    // }
  },
  created () {    
    if (this.$store.state.token) {
      this.getRecommended()
      this.isLoginUser = true
      // console.log(this.RecommendedMovieItems)
    }
    this.getMovieItems()
    // this.toggleLoading()
    // console.log(
    //   this.nowPlayingMovieItems,
    //   this.popularMovieItems,
    //   this.PlaylistMovieItems,
    //   this.RecommendedMovieItems
    // )
    this.handdler()
  },
  mounted () {
		// window.addEventListener('resize', this.handdler)
	}
}
</script>

<style>
body {
  overflow-y: scroll;
  scroll-behavior: smooth;
  scroll-snap-type: y mandatory;
}

section {
  scroll-snap-align: center;
}

</style>