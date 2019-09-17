<template>
  <div id="app">
    <router-view :top_stories="top_stories" :stories="stories"  v-if="show"/>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      stories: [],
      top_stories: [],
      show: false
    };
  },
  mounted() {
    this.getStoriesData();
  },
  methods: {
    // async getStoriesData() {
    //   try {
    //     console.log("23");
    //     var url = "/api/4/news/latest";
    //     let res = await this.$http("https://news-at.zhihu.com" + url);
    //     res = res.stories;
    //     console.log(res);
    //     this.stories = res;
    //   } catch (error) {
    //     console.log(error);
    //   }
    // }
    getStoriesData() {
      var url = "/4/news/latest"; //这里是拼接url
      return axios
        .get("/api" + url) // 这里就是刚才的config/index.js中的/api
        .then(res => {
          if (res.status == 200) {
            // console.log(res);
            
            // res.json(res.data.top_stories)
            this.$nextTick(() => {
              // this.stories = res.data.stories;
              this.stories = res.data.stories;
              this.top_stories = res.data.top_stories;
              this.show = !this.show
            });
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>
<style>

</style>
