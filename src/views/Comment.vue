 <template>
  <div class="comment">
    <div class="comment-header">
      <div class="left">
        <span class="fa fa-mail-reply"></span>
        <span>
          <i>22</i> 条评论
        </span>
      </div>
      <div class="right">
        <span class="fa fa-file-text"></span>
      </div>
    </div>
    <div class="short_comments">
      <div class="top">
        <span>4</span>条短评
      </div>
      <div class="body">
        <ul>
          <li class="liBox" v-for="(item,index) in shortComments" :key="index">
            <div class="img">
              <img :src="item.avatar" alt="">
            </div>
            <div class="content-box">
              <div class="content_top">
                <h2>{{item.author}} </h2>
                <span class="fa fa-thumbs-up"> <i>{{item.likes}} </i> </span>
              </div>
              <div class="content">
                  {{item.content}}
              </div>
              <div class="time">{{item.time}}

              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
 
 <script>
import axios from "axios";
export default {
  name: "Comment",

  data() {
    return {
      shortComments: null,
      parmasId: 0
    };
  },
  methods: {
    getId() {
      this.parmasId = this.$route.params.id;

      //   console.log(this.$route.params.id)
    },
    getStoriesData() {
      var url = "/4/story/" + this.parmasId + "/short-comments"; //这里是拼接url
      return axios
        .get("/api" + url) // 这里就是刚才的config/index.js中的/api
        .then(res => {
          if (res.status == 200) {
            console.log(res);
            this.shortComments = res.data.comments;
            this.$nextTick(() => {});
            console.log(this.shortComments);
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  },
  mounted() {
    this.getId();
    this.getStoriesData();
  }
};
</script>
 <style lang="scss">
.comment-header {
  width: 100%;
  height: $h-height;
  line-height: $h-height;
  background: $h-blue;
  display: flex;
  align-items: center;
  justify-content: space-between;
  .left {
    span {
      color: white;
      margin-left: 20px;
    }
  }
  .right {
    span {
      color: white;
      margin-right: 20px;
    }
  }
}
.short_comments {
  .top {
    padding: 15px;
    border-top: 1px solid lightgrey;
    border-bottom: 1px solid lightgrey;
  }
  .body {
    padding: 15px;
    .liBox {
      display: flex;
      padding-top: 15px;
      padding-bottom: 15px;

      border-bottom: 1px solid lightgrey;
      .img {
          flex: 0 0 36px;
        width: 36px;
        height: 36px;
        border-radius: 50%;
        // background: red;
        overflow: hidden;
        img{
            width: 36px;
            height: 36px;
        }
      }
      .content-box {
          padding-left: 5px;
          flex: 1;
          .content_top{
              display: flex;
               align-items: center;
  justify-content: space-between;

          }
      }
    }
  }
}
</style>