<template>
  <div class="Search">
    <el-row>
      <el-col :span="4" :offset="2">
        <router-link to='/'><img class="logo" src="../../static/img/car.png"  width="50%"></router-link>
      </el-col>
      <el-col :span="4" :offset="0">
        <router-link to='/'><img class="text" src="../../static/img/text.png"  width="80%"></router-link>
      </el-col>
      <el-col :span="8" :offset="0">
        <div class="inpbtn">
          <input type="text" v-model="serachText">
          <button @click="SearchText">搜索</button>
        </div>
      </el-col>
      <el-col :span="4">
        <span class="userImg" @click="gotoMine">
          <img :src="imgPath">
        </span>
        <span class="userName" v-if="isNotLogin" @click="gotoLogin('login')">登录</span>
        <span class="userName" v-if="isNotLogin" @click="gotoLogin('register')">注册</span>
        <span class="userName" v-if="!isNotLogin" @click="gotoMine">{{userInfo.username}}</span>
        <span class="userName" v-if="!isNotLogin" @click="gooutLogin()">退出登录</span>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: 0,
      userInfo: '',
      serachText: '',//带参查询
      isNotLogin: true,
      imgPath: "../../static/img/akari.jpg",
    }
  },
  created(){
    this.id = this.$store.state.userid;
    console.log(this.id)
    //加载用户登录信息
    if(this.id != 0){
      this.$axios.get('/user/selectUserById', {
        params: {
          id : this.$store.state.userid
        }
      }).then(res => {
        this.userInfo = res.data;
        this.isNotLogin = false;
        if(res.data.userImg != null){
          this.imgPath = res.data.userImg;
        }
      })
    }
  },
  methods: {
    //跳转我的账户页面
    gotoMine () {
      //判断用户是否登录
      if(this.id == 0){
        this.$message({
          message: '请登录',
          type: 'warning'
        });
      }else{
        this.$router.push({ name: 'mine' });
      }
    },
    //跳转登陆页面
    gotoLogin(data){
      this.$router.push({
        name: 'login',
        query: {
          name: data
        },
      });
    },
    //退出登录
    gooutLogin(){
      this.$store.commit('userInfo',0);
      this.$router.push({
        name: 'login',
        query: {
          name: 'login'
        },
      });
    },
    SearchText(){
      this.$router.push({
        name: 'productType',
        query: {
          id: 0,
          serachText:this.serachText
        },
      });
      
      this.$router.go(0)
    }
  }
}
</script>

<style scoped="scoped">
.Search {
  height: 100px;
  /* background-color: rgba(0, 0, 0, 0.829); */
  background-color: rgb(88, 71, 166);
  box-shadow: 0 1px 2px #e0e0e0;
  margin-bottom: 20px;
  /* margin-left: 10%;
  margin-right: 10%; */
  border-radius: 0px 0px 25px 25px;
  box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.2);
}
.logo{
  margin-top: -20px;
} 
.inpbtn {
  height: 20px;
  margin-top: 30px;
  display: flex;
  margin-left: -100px;
  width: 700px;
}
.inpbtn input {
  width: 55%;
  height: 40px;
  padding: 0 10px;
  border: 2px solid rgb(88, 71, 166);
  box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.7);
  border-radius: 10px;
}
.inpbtn button {
  width: 15%;
  height: 44px;
  color: #ffffff;
  font-size: 16px;
  font-weight: 800;
  cursor: pointer;
  background-color: rgb(93, 75, 174);
  margin-left: 10px;
}
.inpbtn button:hover{
  background-color: #573580;
}
.userImg {
  display: block;
  float: left;
  margin-right: 10px;
  margin-top: 22px;
  cursor: pointer;
}
.userImg img {
  height: 48px;
  width: 48px;
  border-radius: 50%;
  border: 2px solid #EEEEEE;
}
.userName {
  line-height: 100px;
  margin-left: 10px;
  color: #ffffff;
  cursor: pointer;
}
.shopCart {
  line-height: 100px;
  color: #444;
  margin-left: 20px;
  cursor: pointer;
}

.text {
  margin-left: -230px;
  margin-top: -65px;
}
</style>
