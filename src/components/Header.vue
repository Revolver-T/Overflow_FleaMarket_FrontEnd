<template>
  <div class="header">
    <el-row>

        <div class="lore">
          <el-link :underline="true" href="/">首页</el-link>|
          <el-link :underline="true" @click="gotoMyPage()">我的主页</el-link>
          <!-- <h5 @click="gotoLatest()">最新上架</h5> -->
        </div>

    </el-row>
  </div>
</template>

<script>
export default {
  data:{

  },
  created(){

  },
  methods: {
    // gotoLatest(){
    //   this.$router.push('/LatestProduct');
    // }

    // this.$router.push('/')

    gotoMypage(){
      console.log('233')
      this.$router.push("/mine")
    }

  }
}
</script>

<style scoped="scoped">
a {
  text-decoration: none;
}
.header {
  height: 70px;
  width: 100%;
  line-height: 30px;
  color: #666;
  font-size: 20px;
  background-color: #ffffff;
  border-bottom: 1px solid #eee;
  margin-top: 5px;
  margin-left: 20px;
  margin-right: 20px;
  box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
}
.header a {
  transition: color 0.1s;
}
.header a:hover {
  color: rgb(40, 37, 94);
}
.lore {
  color: rgb(0, 0, 0);
  /* margin-top: -50px; */
  height: 50px;
  width: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  /* margin-top: -3.5%; */
}
.lore .el-link {
  margin: 0 10px;
  color: #555555;
}
</style>
