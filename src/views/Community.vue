<template>
<div class="buster-light">

<div class="hero common-hero">
	<div class="container">
		<div class="row">
			<div class="col-md-12">
				<div class="hero-ct">
					
					<h1>Community</h1>
					<ul class="breadcumb">
						<li @click.prevent="$router.push({name: 'Home'})" class="active"><a href="#">Home</a></li>
						<li><span class="ion-ios-arrow-right"></span> Community</li>
					</ul>
				</div>
			</div>
		</div>
	</div>
</div>
<!-- blog grid section-->
<div class="page-single">
	<div class="container">
		<div class="row">
			<div class="col-md-9 col-sm-12 col-xs-12">				
        <review-item v-for="(review, index) in reviews" :key="index" :review="review"></review-item>
			</div>
			<div class="col-md-3 col-sm-12 col-xs-12" style="position: sticky; top: 100px;">
        <community-sidebar></community-sidebar>		
			</div>
		</div>
	</div>
</div>
		<infinite-loading @infinite="getReviews" spinner="waveDots" :distance="700" ></infinite-loading>
		</div>
</template>

<script>
import axios from 'axios'
import InfiniteLoading from 'vue-infinite-loading';
// import SkeletonBox from '../components/SkeletonBox.vue'
import CommunitySidebar from '../components/Community/CommunitySidebar.vue'
import ReviewItem from '../components/Community/ReviewItem.vue'
const API = process.env.VUE_APP_BACKEND_URL

export default {
  components: { 
    CommunitySidebar,
    ReviewItem,
		InfiniteLoading,
    // SkeletonBox
  },
  name: 'Community',
  data () {
    return {
      page: 1,
      reviews: [],
      isLoading: false
    }
  },
	methods: {
    getReviews ($state) {
      this.isLoading = true 
      axios({
				method: 'get',
				url: `${API}/api/v1/community/reviewlist/${this.page}/`
			})
				
      .then((res) => {        
        if (res.data.length) {
          // console.log(this.page, res.data)
          this.page += 1;
          // this.$store.dispatch('getMovies', this.page)            
          this.reviews.push(...res.data)
          // console.log(this.reviews)
          $state.loaded();
          this.isLoading = false
        } else {
          $state.complete();
          this.isLoading = false
        }        
      }

      )
      .catch((err) => {
        console.log(err)
        this.isLoading = false
      })
    },
    getFirst () {
      axios({
				method: 'get',
				url: `${API}/api/v1/community/reviewlist/${this.page}/`
			})

      .then((res) => {
        this.page += 1
        this.reviews.push(...res.data)
      })
    }    
  },
	created () {
    // this.$emit('infinite')    
    // this.getReviews()
    // axios({
    //   method: 'get',
    //   url: `${API}/api/v1/community/reviewlist/${this.page}/`
    // })
      
    // .then((res) => {
    //   if (res.data.length) {
    //     this.page += 1;
    //     this.reviews.push(...res.data)
    //   }
    // })
	},  
}
</script>

<style scoped>
.grid { 
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-gap: 20px;
  align-items: stretch;
  }
.grid > article {
  border: 1px solid #ccc;
  box-shadow: 2px 2px 6px 0px  rgba(0,0,0,0.3);
}
.grid > article img {
  max-width: 100%;
}
.text {
  padding: 0 20px 20px;
}
.text > button {
  background: gray;
  border: 0;
  color: white;
  padding: 10px;
  width: 100%;
  }

@media screen and (max-width: 321px) {
  #card {
    height: 70vh;
  }
}
@media screen and (min-width: 321px) and (max-width: 376px) {
  #card {
    height: 80vh;
  }
  /* #img {
    height: 100%;
  } */
  
}
@media screen and (min-width: 376px) and (max-width: 992px) {
  #card {
    height: 85vh;
  }    
}
@media screen and (min-width: 992px) and (max-width: 1200px) {
  #card {
    /* height: 70h; */
  }
}
@media screen and (min-width: 1200px) {
  #card {
    height: 55vh;
  }
}
</style>