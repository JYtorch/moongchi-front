<template>
  <div class="buster-light">
<div class="hero common-hero">
	<div class="container">
		<div class="row">
			<div class="col-md-12">
				<div class="hero-ct">
					<h1>Movie Listing</h1>
					<ul class="breadcumb">
						<li @click.prevent="$router.push({name: 'Home'})" class="active"><a href="#">Home</a></li>
						<li> <span class="ion-ios-arrow-right"></span> movie listing</li>
					</ul>
				</div>
			</div>
		</div>
	</div>
</div>
<div class="page-single">
	<div class="container">
		<div class="row">
			<div class="col-md-12 col-sm-12 col-xs-12">
				<div class="topbar-filter fw">
					<p>Found <span>{{ movies.length }} movies</span> in total</p>
					
					<label v-if="!mobileSize">Sort by:</label>
					<select v-if="!mobileSize" disabled>
						<option v-if="!mobileSize" value="popularity" style="color: grey;">Popularity Descending</option>
						<option v-if="!mobileSize" value="popularity" style="color: grey;">Popularity Ascending</option>
						<option v-if="!mobileSize" value="rating" style="color: grey;">Rating Descending</option>
						<option v-if="!mobileSize" value="rating" style="color: grey;">Rating Ascending</option>
						<option v-if="!mobileSize" value="date" style="color: grey;">Release date Descending</option>
						<option v-if="!mobileSize" value="date" style="color: grey;">Release date Ascending</option>
					</select>
					<a v-if="!mobileSize" @click.prevent="" href="" class="list"><i class="ion-ios-list-outline "></i></a>
					<a v-if="!mobileSize" @click.prevent="" href="" class="grid"><i class="ion-grid active"></i></a>
					
				</div>
				<div class="flex-wrap-movielist mv-grid-fw">
            	<movie-list-item v-for="movie in movies" :key="movie.id" :movie="movie"></movie-list-item>									
				</div>
        

		  		<infinite-loading @infinite="getMovies">					
					<div slot="spinner" class="flex-wrap-movielist mv-grid-fw">
						<div v-for="i in Array(30).fill(0).map((e, i) => i + 1)" :key="i">
							<div class="movie-item-style-2 movie-item-style-1">
								<div style="width: 170px; height: 255px;">
									<skeleton-box :width="'100%'" :height="'100%'"></skeleton-box>
								</div>
							</div>
						</div>
					</div>
					<div slot="no-more">No more message</div>					
				</infinite-loading>
				<div class="topbar-filter">
          
					<label>Movies per page:</label>
					<select>
						<option value="range">20 Movies</option>
						<option value="saab">10 Movies</option>
					</select>
				</div>	

			</div>
		</div>
	</div>
</div>
		</div>
</template>

<script>
import InfiniteLoading from 'vue-infinite-loading';
import SkeletonBox from '../components/SkeletonBox.vue'
import axios from 'axios'
import MovieListItem from '../components/MovieList/MovieListItem.vue'
const API = process.env.VUE_APP_BACKEND_URL

export default {
  components: { 
		MovieListItem, 
		InfiniteLoading,
		SkeletonBox
	},
  name: 'MovieList',
  data () {
    return {
		page: 1,
      	movies: [],
		mobileSize: false,		
    }
  },
  methods: {
    getMovies ($state) {      
      axios({
				method: 'get',
				url: `${API}/api/v1/movies/movielist/${this.page}`
			})
				
				.then((res) => {   
        
        if (res.data.length) {
          this.page += 1;

          // this.$store.dispatch('getMovies', this.page)
          this.movies.push(...res.data)
					// console.log(this.movies)
          $state.loaded();
        } else {
          $state.complete();
        }

      });
    },
		handdler () {
			if (window.innerWidth < 768) {
				this.mobileSize = true
			} else {
				this.mobileSize = false
			}
		}
  },
	mounted () {
		if (window.innerWidth < 768) {
			this.mobileSize = true
		}
		window.addEventListener('resize', this.handdler)
	}
}
</script>

<style>

</style>