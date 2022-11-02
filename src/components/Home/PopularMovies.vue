<template>

<div class="col-md-12">
	<div class="title-hd">
		<h2>인기작</h2>
		<a @click.prevent="$router.push({name: 'MovieList'})" href="#" class="viewall">View all <i class="ion-ios-arrow-right"></i></a>
	</div>
	<div class="row">
		<carousel-3d v-if="isRendered && !isLoading" :disable3d="true" :space="395" :autoplay="true" :controlsVisible="true" :height="480">
			<slide v-for="(slide, i) in slides" :index="i" :key="i">
				<template slot-scope="{ index, isCurrent, leftIndex, rightIndex }">												
          <div class="movie-item" style="width: 100%; height: 100%; position:relative; z-index:9998;">
            <img :data-index="index" :class="{ current: isCurrent, onLeft: (leftIndex >= 0), onRight: (rightIndex >= 0) }" :src="`https://image.tmdb.org/t/p/original${slide.poster_path}`">
            <div class="hvr-inner">
              <a @click.prevent="$router.push({ name: 'MovieDetail', params: { id: `${slide.id}` }})" href="moviesingle.html"> Read more <i class="ion-android-arrow-dropright"></i> </a>
            </div>
          
            <div class="title-in" style="padding: 1em; background-color: rgba( 0, 0, 0, 0.5 ); width: 100%; margin: 0 0;">    
              <h6 style="margin-left: 30px;"><a @click.prevent="$router.push({ name: 'MovieDetail', params: { id: `${slide.id}` }})" href="#" style="font-size: 20px;">{{ slide.title }}</a></h6>
              <p style="margin-left: 30px;"><i class="ion-android-star"></i><span>{{ Math.round(slide.rating_average * 10) / 10 }}</span> /10</p>
            </div>
          </div>
				</template>
			</slide>
		</carousel-3d>
    <carousel-3d v-show="!isRendered || isLoading" :disable3d="true" :space="395" :display="20" :controlsVisible="true" :height="480" :clickable="false">
			<slide v-for="(slide, i) in 3" :index="i" :key="i" style="border-radius: 10px; border-width: 0px">
				<skeleton-box :height="'100%'" :width="'100%'" style="position: absolute" />
			</slide>
		</carousel-3d>
	</div>
	<hr>
	<br>
</div>
		

</template>

<script>
import { Carousel3d, Slide } from 'vue-carousel-3d';
import SkeletonBox from '../SkeletonBox.vue'
export default {
  components: {
		Carousel3d,
    Slide,
    SkeletonBox
  },
  name: 'MovieItems',
  props: {
    slides: Array,
    width: Number
  },
  data () {
    return {
      isRendered: false,
      isLoading: true,
    }
  },
  created () {    
    
  },
  watch: {
		slides () {			
			this.isRendered = true
		},
    isRendered () {
			setTimeout(function () {
				this.isLoading = false
			}.bind(this), 1000)
		}
	}
}
</script>

<style>

</style>