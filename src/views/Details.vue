 <template>
  <div class="box">
    <div class="header">
      <div class="left">
        <i class="fa fa-mail-reply" @click="back"></i>
      </div>
      <div class="right">
        <i class="fa fa-share-alt"></i>
      <router-link :to="`/comment/${$route.params.id}`" tag="em" >
        <i class="fa fa-star-o"></i>
        <i class="fa fa-envelope">0</i>
      </router-link>
        <i class="fa fa-thumbs-up">0</i>
      </div>
    </div>
    <div class="img" v-if="data">
      <img :src="data.image" alt />
    </div>

    <div ref="detail"></div>
  </div>
</template>
 
 <script>
import axios from "axios";
export default {
  name: "Details",
  props: ["id"],
  data() {
    return {
      data: null,
      parmasId: null
    };
  },
  mounted() {
    this.detail();
    this.getStoriesData();
  },
  methods: {
    back() {
      this.$router.back(-1);
    },
    getStoriesData() {
      
      var url = "/4/news/" + this.parmasId; //这里是拼接url
      // console.log(url)
      return axios
        .get("/api" + url) // 这里就是刚才的config/index.js中的/api
        .then(res => {
          if (res.status == 200) {
            console.log(res);
            this.data = res.data;

            this.$nextTick(() => {
              this.$refs.detail.innerHTML = this.data.body;
            });
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    detail() {
      // console.log(this.$refs.detail)
      console.log(this.$route, "详情页面");
      this.parmasId = this.$route.params.id;
      // console.log(this.$route.params.id)
    }
  },
  beforeRouteEnter(to, from, next) {
    next();
  },
  beforeRouteUpdate(to, from, next) {
    console.log("beforeRouteUpdate");
    // let reg=new RegExp(this.path);
    // console.log(reg.test(to.path))
    // // console.log(to.parmas.id)
    // //reg.test(to.path)为真代表父元素  ，为假代表子元素
    // if(!reg.test(to.path)){
    //   this.refreshData(to.params.id)
    //   this.path=to.path

    // }
    next();
  }
};
</script>
 
 <style lang="scss">
.box {
  position: relative;
  width: 100%;
  height: 100%;
  .img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 210px;
    overflow: hidden;
  margin-top: $h-height;
    img {
      width: 100%;
      height: auto;
      margin-top:-100px;
    }
  }
}
.left {
  margin-left: 20px;
}
.right {
  float: right;

  i {
    margin-right: 20px;
    vertical-align: right;
  }
}
</style>