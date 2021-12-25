<template>
  
<div>  
    <div class="comment-box" style="position: inline;">
      <div class="author-box">      
      <!-- <span id="reply-mark">└</span> -->
        <a @click.prevent="$router.push({name: 'UserProfile', params: { username: nestedComment.user.username }})" href=""><h6>{{ nestedComment.user.username }}</h6></a>
        <div class="date-box">
          <span id="time" class="time" :style="{fontSize: size}">
            {{ nestedComment.created_at.slice(0,10) }} {{nestedComment.created_at.slice(11, 19) }}
          </span>
          
          <a id="delete-btn" @click.prevent="deleteNestedComment" href="" class="time">X</a>
          <a id="report-btn" @click.prevent="reportNestedComment" v-show="isLogin" href="">
            <h6 class="time">신고</h6>
          </a>
        </div>
      </div>      

      <div class="content-box">
        <p>{{ nestedComment.content }}</p>
      </div>

      <div class="btn-box">
        

        <div id="like-box">        

          <a id="comment-like-btn" @click.prevent="likeNestedComment" href="" class="time">
            <font-awesome-icon :icon="['far', 'thumbs-up']" size="1x" style="margin-left: 7px; margin-right: 7px;" />
            <strong>{{ nestedComment.like_users.length }}</strong> 
          </a>
          <a id="comment-dlike-btn" @click.prevent="dislikeNestedComment" href="" class="time">
            <font-awesome-icon :icon="['far', 'thumbs-down']" size="1x" style=" margin-right: 7px; margin-left: 7px;" />
            <strong>{{ nestedComment.dlike_users.length }}</strong> 
          </a>          
        </div>
      </div>          
    </div>
  
</div>
    <!-- <div style="margin: 0px 5px;">
      <p style="white-space: pre-line; width: 550px;">{{ nestedComment.content }}</p>
    </div> -->
  
  
</template>

<script>
import axios from 'axios'
const API = process.env.VUE_APP_BACKEND_URL

export default {
  name: 'NestedCommentItems',
  props: {
    nestedComment: Object,
    commentId: Number,
    isLogin: Boolean
  },
  data () {
    return {
      size: null
    }
  },
  methods: {
    setToken: function () {
      const token = localStorage.getItem('jwt')
      const config = {
        Authorization: `JWT ${token}`
      }
      return config
    },

    likeNestedComment () {
      axios({
        method: 'post',
        url: `${API}/api/v1/community/${this.commentId}/nestedcomment/${this.nestedComment.id}/like/`,
        headers: this.setToken() // Authorization: JWT tokensdjiadnoiqwnd
      })
        .then(res => {
          console.log(res)          
          this.$emit('noti-comment')
        })
        .catch(err => {
          alert('로그인이 필요합니다.')
          console.log(err)
        })
    },
    dislikeNestedComment () {
      axios({
        method: 'post',
        url: `${API}/api/v1/community/${this.commentId}/nestedcomment/${this.nestedComment.id}/dlike/`,
        headers: this.setToken() // Authorization: JWT tokensdjiadnoiqwnd
      })
        .then(res => {
          console.log(res)
          this.$emit('noti-comment')
        })
        .catch(err => {
          alert('로그인이 필요합니다.')
          console.log(err)
        })
    },
    deleteNestedComment () {
      axios({
        method: 'delete',
        url: `${API}/api/v1/community/${this.commentId}/nestedcomment/${this.nestedComment.id}/`,
        headers: this.setToken() // Authorization: JWT tokensdjiadnoiqwnd
      })
        .then(res => {
          console.log(res)
          this.$emit('noti-comment')
        })
        .catch(err => {
          console.log(err)
          alert('댓글을 삭제할 권한이 없습니다.')
        })
    },
    reportNestedComment () {
      axios({
        method: 'post',
        url: `${API}/api/v1/reports/nestedcomment/${this.nestedComment.id}/`,        
        headers: this.setToken() // Authorization: JWT tokensdjiadnoiqwnd
      })
        .then(res => {
          console.log(res)
          alert('신고가 접수되었습니다.')
        })
        .catch(err => {
          console.log(err)
          alert('이미 신고된 댓글입니다.')
        })
    }
  },
  created () {
    if (window.innerWidth < 350) {
      this.size = 0.5 + 'em'
    }
  }
}
</script>

<style scoped>
.comment-box {
  margin: 0.5em 0;
  width: 100%;
  padding: 1em;
  background-color: #F1F3F5;
  border-radius: 10px;
  margin-left: 1em;
}
.author-box {
  display: flex;
  justify-content: space-between;  
}
.author-box > h6 {
  padding: 0px 10px;
}
.content-box {   
    /* width: 65%; */
    padding: 0.5em 0;
}

.content-box > p {
  white-space: pre-line; 
  word-break:break-all;
  width: 100%;    
}
#reply-mark {
  position: absolute;
  left: 7px;  
  font-size: 1.8em;
}
.btn-box {  
  /* width: 20%;
  margin-left: 1em;
  position: relative;
  left: 4em; */
}
.date-box {
  /* display: flex;  
  width: 100%;   */
}
.date-box > span {
  width: 70%;
}
.date-box > a {
  
}
#delete-btn {
  border: thin solid; 
  background-color: #F1F3F5;  
  width: 10%;  
  text-align: center;
  margin: 0 0.5em;
}
#time {
  width: auto;
}
#report-btn > h6{
  /* margin-top: 5px; 
  margin-right: 5px; */
  padding-left: 0.1em;
  display: inline;
}

#comment-like-btn {
  border: solid; 
  border-width: thin; 
  border-radius: 2px; 
  margin-right: 5px;
}
#comment-like-btn > strong {
  font-family: tahoma; 
  color: #777;
  padding: 0 0.5em;
}
#comment-dlike-btn > strong {
  font-family: tahoma; 
  color: #777;
  padding: 0 0.5em;
}
#comment-dlike-btn {
  font-weight: bolder; 
  border: solid; 
  border-width: thin; 
  border-radius: 2px;
}
#like-box {
  display: flex;
  justify-content: flex-end;
  
  
}

/* @media screen and (min-width: 690px) {
  .author-box {
    width: 15%;
  }
  .content-box {   
    width: 60%;
  }  
  .btn-box {  
    width: 25%;
  }
}
@media screen and (min-width: 657px) and (max-width: 690px) {
  .author-box {
    width: 20%;
  }
  .content-box {   
    width: 50%;
  }  
  .btn-box {  
    width: 30%;
  }
}
@media screen and (min-width: 470px) and (max-width: 657px) {
  .author-box {
    width: 25%;
  }
  .content-box {   
    width: 55%;
  }  
  .btn-box {  
    width: 20%;
  }
  .date-box {
    display: flex;
    
  }
  #delete-btn {
    height: 50%;
    width: 20%;
  }
  #time {
    font-size: 0.7em;
  }
  #comment-like-btn {
    text-align: center;
  }
  #comment-dlike-btn {
    text-align: center;
  }
}
@media screen and (min-width: 425px) and (max-width: 470px) {
  .author-box {
    width: 30%;
  }
  .content-box {   
    width: 50%;
  }
  #delete-btn {
    width: 30%;
    height: 60%;
    margin: 0px 0px;
  }
  .btn-box {  
    width: 20%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  #time {
    font-size: 0.6em;
  }

  #like-box {
    display: inline;
  }
  #comment-like-btn {
    display: block;
    width: 70%;
    margin-top: 1rem;
  }
  #comment-dlike-btn {
    display: block;
    width: 70%;
    margin: 1rem 0em;
  }
}
@media screen and (min-width: 375px) and (max-width: 425px) {
  .author-box {
    position: relative;
    right: 1em;
    width: 30%;
  }
  .content-box {
    width: 50%;
  }
  .btn-box {
    width: 20%;
  }
  #time {
    font-size: 0.5em;    
    position: relative;
    right: 10px;
  }
  .btn-box {
    display: inline;
  }
  #delete-btn {
    margin: 0px 0px;
    width: 30%;
    height: 50%;
  }
  #comment-like-btn {
   text-align: center; 
  }
  #comment-dlike-btn {
    text-align: center;
  }
}
@media screen and (max-width: 375px) {
  .author-box {
    
    position: relative;
    right: 1.5em;
    width: 20%;
  }
  .author-box > a > h6 {
    width: 6em;
  }
  .content-box {
    width: 50%;
  }
  .btn-box {
    width: 30%;
  }
  .date-box {
    width: auto;
  }
  #time {
    font-size: 5px;    
    position: relative;
    right: 1em;
    width: 58px;
  }
  .btn-box {
    display: inline;
  }
  #delete-btn {  
    position: relative;  
    left: 1em;
    margin: 0px 0px;
    width: 20%;
    height: 50%;
  }
  #comment-like-btn {
   text-align: center; 
  }
  #comment-dlike-btn {
    text-align: center;
  }
  .author-box > a > h6 {
    font-size: 0.9em;
  }
  .content-box > p {
    font-size: 0.8em;
  }
} */
</style>