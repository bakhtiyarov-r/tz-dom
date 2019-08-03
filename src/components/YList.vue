<template>
  <div>  
    
    <div class="container" >
      <div class="video-list-wrap">
        <h1>{{msg}} "{{query}}"</h1>
        <div class="video-list">
          <div class="video-list__elem video-item" v-for="item in displayedVideos">
            <a class="video-item__link" :href="'https://www.youtube.com/watch?v=' + item.id.videoId">
              <div class="video-item__thumb">
                <img :src="item.snippet.thumbnails.medium.url"/>
              </div>
              <div class="video-item__title">
                  {{item.snippet.title}}
              </div>
            </a>
          </div>
        </div>
      </div>
    </div>
    
    <div class="container">
      <pagination v-bind:maxResults="maxResults" v-bind:perPage="perPage" v-on:click="getPage">
      </pagination>
    </div>

    <transition name="fade">
      <preloader v-bind:class="{'is-loaded': isLoaded}" v-if="isLoading"></preloader>
    </transition>
  </div>
</template>

<script>
  import Pagination from './Pagination.vue'
  import Preloader from './Preloader.vue'

  export default {
    components: {
      Preloader,
      Pagination
    },
    props: {
      msg: String
    },
    data() {
      return {
        videoList: [],
        url: 'https://www.googleapis.com/youtube/v3/search?',
        key: 'AIzaSyA1MaBLtS9fiCU0PpAJH4GSLioQaJuSgSM',
        part: 'snippet',
        type: 'video',
        query: 'HTML',
        maxResults: 20,
        perPage: 5,
        page: 1,
        isLoading: true,
        isLoaded: false,
      }
    },
    beforeCreate() {
      
    },
    created() {
      this.route();
      this.getVideo();
      
    },
    mounted() {
      this.hidePreloader();
    },
    methods: {
      getVideo() {
        fetch(`${this.url}key=${this.key}&part=${this.part}&type=${this.type}&maxResults=${this.maxResults}&q=${this.query}`)
        .then((response) => {                
            return response.json();
        })
        .then((data) => {                 
          this.videoList = data.items;
          this.isLoaded = true;
        })
        .catch((error) => {
          console.log(error);
        })
      },
      getPage(page) {
        this.page = page;
      },
      route() {
        this.page = window.location.hash ? window.location.hash.split('#page-')[1] : this.page;
      },
      hidePreloader() {
        setTimeout(() => {
          this.isLoading = false;
        }, 2000)
      }
    },
    computed: {
      displayedVideos() {
        let start = this.page * this.perPage - this.perPage;
        let end = this.page * this.perPage;
        return this.videoList.slice(start, end);
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-leave-to {
    opacity: 0;
  }
  .container {
    width: 90%;
    max-width: 1200px;
    margin: auto;
  }
  .video-list-wrap {
    display: flex;
    flex-direction: column;
    border: 1px solid #97b7ca;
    border-radius: 10px;
    background: linear-gradient(to top, #fefcea, #dce9f1);
    padding: 30px 20px;
  }
  .video-list {
    display: flex;
    flex-wrap: wrap;
  }
  .video-list__elem {
    margin: 10px;
    max-width: 45%;
  }
  .video-item {
  }
  .video-item__link {
    display: flex;
  }

  .video-item__thumb {
    margin: 0 20px 0 0;
    flex: 1 1 30%;
  }

  .video-item__thumb img{
    width: 100%;

  }

  .video-item__title {
    flex: 1 1 70%;
  }


  @media (max-width: 992px) {
    .container {
      width: 90%;
      margin: auto;
      padding: 30px 20px;
    }
    .video-list__elem {
      width: 100%;
      max-width: none;
    }
  }
</style>
