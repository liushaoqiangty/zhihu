 <template>
  <div class="login">
    <div class="login-header">
      <span class="fa fa-mail-reply" @click="backSpace"></span>
      <span>登录</span>
    </div>
    <div class="login-body">
      <div class="zhihu">知乎日报</div>
      <p>
        <!-- 账号: -->
        <input placeholder="请输入账号" />
      </p>
      <p>
        <!-- 密码: -->
        <input placeholder="请输入密码" />
      </p>
      <p>
        <button @click="goto">注册/登录</button>
      </p>
      <router-link to="/enter"><div >已有账号，直接登录</div></router-link>
      <div class="other">
        <p class="weibo">使用微博登录</p>
        <div class="other-login">
          <p>
            <button>新浪微博</button>
          </p>
          <p>
            <button>腾讯微博</button>
          </p>
        </div>
      </div>
    </div>
    <div class="agreement">
      登录即代表你同意
      <span>《知乎协议》</span>和
      <span>《隐私保护协议》</span>
    </div>
  </div>
</template>
 
 <script>
export default {
  name: "Login",
  methods: {
    backSpace() {
      this.$router.back(-1);
    },
    goto(){
       this.$router.replace("/enter");
       console.log("go")
    }
  }
};
</script>
 
<style lang="scss" scoped>
.login {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 119, 255, 0.5);
  // background-color: travelbalck(0.3);
  backdrop-filter: blur(5px);


  z-index: 1;
  .login-header {
    width: 100%;
    height: $h-height;
    line-height: $h-height;
    background: $h-blue;
    span {
      color: white;
      margin-left: 20px;
    }
  }
  .login-body {
    width: 100%;
    height: 100%;
    text-align: center;
    .zhihu {
      width: 100%;
      height: 100px;
      line-height: 100px;
      // margin-top: 50px;
      font-size: 30px;
      color: white;
    }
    .weibo {
      color: rgb(238, 236, 236);
    }
    input {
      width: 80%;
      height: 40px;
      outline: none;
      border: 0;
    }
    button {
      width:40%;
      height: 50px;
      outline: none;
      border: 0;
      background: white;
      font-size: 15px;
      border-radius: 5px;
    }
    .other {
      margin-top: 100px;

      .other-login {
        display: flex;
        p {
          flex: 1;
          button {
            // display: flex;
            width: 100px;
            height: 50px;
            outline: none;
            border: 0;
            background: white;
            font-size: 15px;
            border-radius: 5px;
          }
        }
      }
    }
  }
}
.agreement {
  position: fixed;
  bottom: 0;
  font-size: 10px;
  width: 100%;
  text-align: center;
  color: lightblue;
  font-weight: 100;
  span {
    color: white;
  }
}
</style>"