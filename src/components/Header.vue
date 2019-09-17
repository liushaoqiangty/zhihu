<template>
  <div>
    <div class="header">
      <div class="left">
        <span class="fa fa-navicon" @click="changeShow"></span>
        <span>首页</span>
      </div>
      <div class="right">
      <router-link to="/login" tag="span">
        <span class="fa fa-bell-o"></span>
      </router-link>

        <span class="fa fa-ellipsis-v"></span>
      </div>
    </div>
    <div class="cover" @click.stop="changeShow" v-show="show">
      <transition name="move">
        <div class="aside" @click.stop="showTrue">
        <div class="top">
          <router-link to="/login" tag="div">
            <div class="login">
              <span class="head"></span>
              <span>请登录</span>
            </div>
          </router-link>
          <div class="like">
            <div class="left">
              <span class="fa fa-star-o"></span>
              <span>我的收藏</span>
            </div>
            <div class="right">
              <span class="fa fa-anchor"></span>
              <span>我的下载</span>
            </div>
          </div>
        </div>
        <div class="list">
          <ul>
            <li class="list-item" @click.stop="changeShow">
              <span class="fa fa-bank"></span>
              <span>首页</span>
            </li>
          </ul>
        </div>
      </div>
      
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      show: false
    };
  },
  methods: {
    changeShow() {
      this.show = !this.show;
    },
    showTrue() {
      this.show = true;
    }
  }
};
</script>

<style lang="scss">
.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  width: 100%;
  height: $h-height;
  background: $h-blue;

  line-height: $h-height;
  z-index: 10;
  .left {
  float: left;
  span {
    margin-left: 20px;
  }
}
.right {
  float: right;
  span {
    margin-right: 20px;
  }
}
}

.cover {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.5);
  // filter: blur(10px);
  //  backdrop-filter: blur(5px);
  z-index: 10;
  .move-enter-to{
    transform: translateX(0)
  }
  .move-enter-active{
    transition: all .5s linear;
  }
  .move-enter{
    transform: translateX(100%)
  }
  .aside {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    z-index: 20;
    color: white;
    width: 80%;
    background: white;
    .top {
      position: relative;
      width: 100%;
      height: 100px;
      padding: 10px;
      background: $h-blue;
    }
    .login {
      display: flex;
      height: 40px;
     
      line-height: 60px;
      span:last-child {
        margin-top: -10px;
      }
    }
    .head {
      display: inline-block;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: lightgrey;
    }
    .like {
      margin-top: 20px;
      // position: absolute;
      // bottom: 10px;
      display: flex;
      font-weight: 700;

      div {
        flex: 1;
        
      }
      span{
        margin-left: 10px;
      }
    }
    .list {
      width: 100%;

      .list-item {
        width: 100%;
        height: 50px;
        background: #f0f0f0;
        padding: 15px;
        span {
          margin-left: 15px;
          color: $h-blue;
        }
      }
    }
  }
}
</style>