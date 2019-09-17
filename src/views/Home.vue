<template>
  <div class="home">
    <Header />
    <div class="top_stories">
      <swiper :options="swiperOption" ref="ul">
        <swiper-slide class="swiper-slide" v-for="(item,index) in top_stories" :key="index">
          <router-link :to="`/details/${item.id}`" tag="div">
            <img class="picture" :src="item.image" alt />
            <span class="desc">{{item.title}}</span>
          </router-link>
        </swiper-slide>
        <div class="swiper-pagination" slot="pagination"></div>
        <!-- <swiper-slide class="swiper-slide">
          <img class="picture" :src="top_stories[0].image" alt />
          <span  class="desc">{{top_stories[0].title}}</span>
        </swiper-slide>-->
      </swiper>
    </div>
    <!-- <li>{{top_stories[0].image}}</li> -->
    <!-- <span class="swiper-pagination-bullet swiper-pagination-bullet-active" tabindex="0" role="button" aria-label="Go to slide 3"></span> -->
    <div class="stories">
      <div class="container">
        <div class="today">
          <p>今日要闻</p>
          <ul>
            <li class="liBox" v-for="(item,index) in stories" :key="index">
              <router-link :to="`/details/${item.id}`" tag="div">
                <span class="title">{{item.title}}</span>
                <img class="img" :src="item.images" alt />
              </router-link>
            </li>
          </ul>
        </div>
        <div class="yesterday" v-for="(i,index) in oldTime" :key="index">
          <p>{{i}}</p>
          <ul>
            <li class="liBox" v-for="(item,ln) in oldStories[index]" :key="ln">
              <router-link :to="`/details/${item.id}`" tag="div">
                <span class="title">{{item.title}}</span>
                <img class="img" :src="item.images" alt />
              </router-link>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
import Header from "../components/Header";
let time = new Date();
let y = time.getFullYear();
let m = time.getMonth() + 1;
m = m < 10 ? "0" + m : m;
let d = time.getDate();
let beforetime = y + "" + m + "" + d;
export default {
  name: "home",

  props: ["top_stories", "stories"],
  data() {
    return {
      index: 0,
      timer: null,
      oldStories: [],
      oldTime: [],
      swiperOption: {
        spaceBetween: 30,
        centeredSlides: true,
        loop: true,
        autoplay: {
          delay: 2500,
          disableOnInteraction: false
        },
        pagination: {
          el: ".swiper-pagination",
          clickable: true
        }
      }
    };
  },
  methods: {
    // nextPic() {
    //   let width = document.body.offsetWidth;
    //   this.index++;
    //   let offsetX = -this.index * width;
    //   let slider = this.$refs.ul;
    //   this.$refs.ul.style.transition = "all 1s linear";
    //   this.$refs.ul.style.transform = `translateX(${offsetX}px)`;
    // },
    // resetPic() {
    //   let width = document.body.offsetWidth;
    //   if (this.index >= this.top_stories.length - 1) {
    //     this.index = 0;
    //     // let offsetX = -this.index * width;
    //     // this.$refs.ul.style.transition = "none";
    //     // this.$refs.ul.style.transform = `translateX(${offsetX}px)`;
    //   }
    // },
    getStoriesData() {
      beforetime--;
      let url = `/4/news/before/${beforetime}`; //这里是拼接url
      return axios
        .get("/api" + url) // 这里就是刚才的config/index.js中的/api
        .then(res => {
          if (res.status == 200) {
            // console.log(res);

            this.$nextTick(() => {
              // this.stories = res.data.stories;
              console.log(this.oldTime);
            
                this.oldStories.push(res.data.stories);
                console.log(this.oldStories[2]);
              
              this.oldTime.push(beforetime);
              // console.log(beforetime);
              // console.log(res.data);
              // console.log(res.data.date);
              // console.log(this.oldTime);
              //  this.oldTime.push(res.data.data);
            });
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    scroll() {
      let isLoading = false;

      console.log(beforetime);
      window.onscroll = () => {
        // 距离底部200px时加载一次
        let bottomOfWindow =
          document.documentElement.offsetHeight -
            document.documentElement.scrollTop -
            window.innerHeight <=
          50;
        if (bottomOfWindow && isLoading == false) {
          isLoading = true;
          beforetime--;
          console.log("触底了");
          console.log(beforetime);
          let url = `/4/news/before/${beforetime}`; //这里是拼接url
          return axios
            .get("/api" + url) // 这里就是刚才的config/index.js中的/api
            .then(res => {
              if (res.status == 200) {
                // console.log(res);
                this.$nextTick(() => {
                  // this.stories = res.data.stories;
                  for (var i = 0; i < res.data.stories.length; i++) {
                    this.oldStories.push(res.data.stories[i]);
                  }
                  // this.oldStories = this.oldStories+res.data.stories;
                  console.log(res.data.stories[0]);
                });
              }
            })
            .catch(function(error) {
              console.log(error);
            });
        }
      };
    },
    onScroll(e) {
      if (this.loading || this.allLoaded) return;
      let top = document.documentElement.scrollTop || document.body.scrollTop; // 滚动条在Y轴上的滚动距离
      let vh =
        document.compatMode == "CSS1Compat"
          ? document.documentElement.clientHeight
          : document.body.clientHeight; // 浏览器视口的高度
      let height = Math.max(
        document.body.scrollHeight,
        document.documentElement.scrollHeight
      ); // 文档的总高度
      if (top + vh >= height) {
        // 滚动到底部
        console.log("触底了");

        this.getStoriesData(); // 如果已经滚到底了 获取数据
      }
    }
  },
  created() {
    this.getStoriesData();
    window.addEventListener("scroll", this.onScroll);
  },
  destroyed() {
    window.removeEventListener("scroll", this.onScroll);
  },
  mounted() {
    // this.slow();
    // this.getStoriesData();
    // this.timer = setInterval(this.nextPic, 2000);
    // this.$refs.ul.addEventListener("transitionend", this.resetPic);
  },
  beforeDestroy() {
    //清除mounted中的定时器
    // clearInterval(this.timer);
    // this.$refs.ul.removeEventListener("transitionend", this.resetPic);
  },

  components: {
    Header
  }
};
</script>

<style lang="scss">
.top_stories {
  // margin-top: 50px;
  width: 100%;
  height: 300px;
  overflow: hidden;

  // background: red;
  .swiper-pagination {
    .swiper-pagination-bullet {
      width: 10px;
      height: 10px;
      background: #88878d;
      opacity: 1;
    }
    .swiper-pagination-bullet-active {
      background: white;
    }
  }
  .swiper-container {
    width: 100%;
    height: 300px;
    .picture {
      width: 100%;
      height: auto;
    }
    .desc {
      position: absolute;
      bottom: 20px;
      left: 0;
      font-size: 25px;
      font-weight: 500;
      color: white;
    }
  }
  // .ul {
  //   width: 1000%;
  //   height: 300px;
  // }
  // .li {
  //   position: relative;
  //   width: 10%;
  //   height: 300px;
  //   float: left;

  // .picture {
  //   width: 100%;
  //   height: 300px;
  // }
  //   .desc {
  //     position: absolute;
  //     bottom: 0;
  //     left: 0;
  //     font-size: 25px;
  //     font-weight: 500;
  //     color: white;
  //   }
  // }
}
.stories {
  width: 100%;
  padding: 10px;
  // padding-top: 0;
  background: lightgray;
  .liBox {
    width: 100%;
    height: 90px;
    box-shadow: 0 0 1px white;
    border-radius: 5px;
    background: white;
    margin-top: 10px;

    padding: 10px;
    div {
      display: flex;
    }
    .title {
      flex: 1;
    }
    .img {
      width: 80px;
    }
  }
  .yesterday {
    margin-top: 10px;
  }
}
</style>
