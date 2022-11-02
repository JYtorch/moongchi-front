<template>
<div class="slider movie-items" style="">
	<div class="container">
		<div class="row">
			<div class="social-link">
				<p>Follow us: </p>
				<a href="https://ko-kr.facebook.com/zuck"><i class="ion-social-facebook"></i></a>
				<a href="https://twitter.com/jack"><i class="ion-social-twitter"></i></a>
				<a href="https://www.youtube.com/c/%EC%8A%B9%EC%9A%B0%EC%95%84%EB%B9%A0"><i class="ion-social-youtube"></i></a>
			</div>
			<carousel-3d v-show="isRendered && !isLoading" :autoplay="true" :display="20" :controlsVisible="true" :height="490" :style="{width: width + 'px'}" >
				<slide v-for="(slide, i) in slides" :index="i" :key="i">      
					<template slot-scope="{ index, isCurrent, leftIndex, rightIndex }">
						<div class="movie-item" style="width: 100%; height: 100%; position:relative; z-index:9998;">									
							<img id="sliderimg" :data-index="index" :class="{ current: isCurrent, onLeft: (leftIndex >= 0), onRight: (rightIndex >= 0) }" :src="`https://image.tmdb.org/t/p/original${slide.poster_path}`" style="">
							<div class="hvr-inner">
								<a @click.prevent="$router.push({ name: 'MovieDetail', params: { id: `${slide.id}`}})" href="moviesingle.html"> Read more <i class="ion-android-arrow-dropright"></i> </a>
							</div>
							<div class="title-in" style="padding: 1em; background-color: rgba( 0, 0, 0, 0.5 ); width: 100%; margin: 0 0;">
								<div class="cate">
									<span class=""><a href="#">{{slide.release_date}}</a></span>
								</div>
								<h6 style="margin-left: 30px;"><a @click.prevent="$router.push({ name: 'MovieDetail', params: { id: `${slide.id}`}})" href="#" style="font-size: 1.5em;">{{ slide.title }}</a></h6>
								<p style="margin-left: 30px;"><i class="ion-android-star"></i><span>{{ Math.round(slide.rating_average * 10) / 10 }}</span> /10</p>
							</div>
						</div>
					</template>
				</slide>					
			</carousel-3d>
			<carousel-3d v-show="!isRendered || isLoading" :display="20" :height="490" :style="{width: width + 'px'}" >
				<slide v-for="(slide, i) in 7" :index="i" :key="i" style="border-width: 0px; border-radius: 10px;">
					<skeleton-box :height="'100%'" :width="'100%'" style="border-radius: 5%; position: absolute; border-radius: 10px;" />
				</slide>					
			</carousel-3d>
	    </div>
	</div>
</div>
</template>

<script>
import { Carousel3d, Slide } from 'vue-carousel-3d';
import SkeletonBox from '../SkeletonBox.vue';

export default {
  	components: { 
			Carousel3d,
			Slide,
    		SkeletonBox
		},
	props: {
		slides: Array,
		width: Number,		
		},
	name: 'SliderMovieItems',
	data () {
		return {			
			isRendered: false,
			isLoading: true,
		}
  	},
	methods: {
		toggleLoading () {
			this.isLoading = false
		}
	},
  	created () {
	
  	},
	mounted () {		
		// window.addEventListener('resize', this.handdler)
	},
	watch: {
		slides () {			
			this.isRendered = true
		},
		isRendered () {
			setTimeout(function () {
				this.isLoading = false
			}.bind(this), 3000)
		}
	}
}
</script>

<style scoped>
img {
	background-image: linear-gradient( rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3) );
}

</style>