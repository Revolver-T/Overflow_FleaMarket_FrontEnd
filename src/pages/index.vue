<template>
  <div class="containerIndex">
    <Search style="margin-top: 0px;"></Search>
    <!-- <HeaderTop></HeaderTop> -->

    <div class="header">
    <el-row>

        <div class="lore">
          <el-link :underline="true" href="/">首页</el-link>|
          <el-link :underline="true" @click="gotoMypage()">我的主页</el-link>|
          <el-link :underline="true" @click="gotoLatest()">最新上架</el-link>
          <!-- <h5 @click="gotoLatest()">最新上架</h5> -->
        </div>

    </el-row>
      </div>

    <el-backtop></el-backtop>
    <!-- 幻灯片-->
    <el-row class="slide">
      <el-col :span="16" :offset="4">
        <!-- 一级列表-->
        <ul class="slideUl">
          <li v-for="item in fc" :key="item.id" @click="gotoType(item.id)">{{item.name}}<span class="slideUlclick"></span></li>
        </ul>
        <div class="block">
          <el-carousel height="100%"  :interval="3000" trigger="click">
            <el-carousel-item v-for="item in Slide" :key="item">
              <img :src="item" alt="">
            </el-carousel-item>
          </el-carousel>
        </div>
      </el-col>
    </el-row>

    <el-divider></el-divider>
    <!-- 热门商品-->
    <el-row class="hot">
      <el-col :span="4" :offset="4">
        <h2>热门商品</h2>
      </el-col>
      <el-col class="center" :span="16" :offset="4">
        <ul class="typeProduct">
          <li v-for="item in hotpro" :key="item.id">
            <div class="containerProduct">
              <div class="imgbox"><img :src="item.images[0]" max-height="100%" :alt="item.name" @click="gotoDetails(item.id)"></div>
              <div class="text" style="position: relative;bottom: 0;height: 50px;">
                <p class="productPrice">¥<span class="price">{{item.price}}</span> </p>
                <p class="productName" @click="gotoDetails(item.id)">{{item.name}}</p>
                <div class="userPlay">
                  <span style="font-size: 12px;color:#bbbcc2;">{{item.createTime}}</span>
                  <span><button @click="addLove(item.id,item.uid)">点击收藏</button></span>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </el-col>
    </el-row>


    <el-divider v-if="this.newpro!==''"></el-divider>
    <!-- 最新上架-->
    <div>
    <div id="latest">
    <el-row class="new" v-if="this.newpro!==''">
      <el-col :span="4" :offset="4">
        <h2>最新上架</h2>
      </el-col>
      <el-col class="center" :span="16" :offset="4">
        <ul class="typeProduct">
          <li v-for="item in newpro" :key="item.id">
            <div class="containerProduct">
              <div class="imgbox"><img :src="item.images[0]" :width="imgwidth" :alt="item.name" @click="gotoDetails(item.id)"></div>
              <div class="text" style="position: relative;bottom: 0;height: 50px;">
                <p class="productPrice">¥<span class="price">{{item.price}}</span> </p>
                <p class="productName" @click="gotoDetails(item.id)">{{item.name}}</p>
                <div class="userPlay">
                  <span style="font-size: 12px;color:#bbbcc2;">{{item.creattime}}</span>
                  <span><button @click="addLove(item.id,item.uid)">点击收藏</button></span>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </el-col>
    </el-row>
    </div>
  </div>

    <el-divider v-if="this.recommendpro!==''"></el-divider>
    <!-- 推荐商品-->
    <el-row class="new" v-if="this.recommendpro!==''">
      <el-col :span="4" :offset="4">
        <h2>全部商品</h2>
      </el-col>
      <el-col class="center" :span="16" :offset="4">
        <ul class="typeProduct">
          <li v-for="item in recommendpro" :key="item.id">
            <div class="containerProduct">
              <div class="imgbox"><img :src="item.images[0]" :width="imgwidth" :alt="item.name" @click="gotoDetails(item.id)"></div>
              <div class="text" style="position: relative;bottom: 0;height: 50px;">
                <p class="productPrice">¥<span class="price">{{item.price}}</span> </p>
                <p class="productName" @click="gotoDetails(item.id)">{{item.name}}</p>
                <div class="userPlay">
                  <span style="font-size: 12px;color:#bbbcc2;">{{item.createTime}}</span>
                  <span><button @click="addLove(item.id,item.uid)">点击收藏</button></span>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </el-col>
    </el-row>
    <FooterBottom></FooterBottom>
  </div>
</template>

<script>
import HeaderTop from "../components/Header.vue";
import Search from "../components/Search.vue";
import FooterBottom from "../components/Footer.vue";
import moment from "moment";

export default {
  components: {
    HeaderTop,
    Search,
    FooterBottom
  },
  data () {
    return {
      Slide: ["../../static/img/slide/QQ图片20230828111534.png",
        "../../static/img/slide/galaxy-s23-highlights-kv.webp",
        "../../static/img/slide/tb_image_share_1693195052040.jpg"],
      count: 1,
      seen: false,
      current: 0,
      imgwidth: "30px",
      fc: '',
      hotpro: '',//热门商品
      newpro: '',//最新商品
      recommendpro: ''//推荐商品
    }
  },
  mounted(){
    this.targetElement = document.getElementById('latest');
  },
  created() {
    //加载一级分类目录
    this.$axios.get('/classify/flnb', {
      params: {}
    }).then(res => {
      this.fc = res.data;
    });

    //加载热门商品
    this.$axios.get('/product/selectByHot', {
      params: {}
    }).then(res => {
      console.log(res);
      console.log(res.data.data);
      res.data.data.forEach((res,index) => {
        if(res.images != null){
          res.images = eval('('+res.images+')');
        }
      });
      this.hotpro = res.data.data;
      // console.log(res.data.data.createTime.substring(1,10))
      // hotpro.createTime = res.data.data.createTime.substring(1,10);
      // console.log(hotpro.createTime)
    })
    //加载最新上架商品
    this.$axios.get('/product/selectByNew', {
      params: {}
    }).then(res => {
      console.log(res);
      console.log(res.data.data);
      res.data.data.forEach((res,index) => {
        if(res.images != null){
          res.images = eval('('+res.images+')');
        }
      });
      this.newpro = res.data.data;
    });
    //加载官方推荐商品
    this.$axios.get('/product/selectByCommend', {
      params: {}
    }).then(res => {
      res.data.data.forEach((res,index) => {
        if(res.images != null){
          res.images = eval('('+res.images+')');
        }
      });
      this.recommendpro = res.data.data;
    });
    //点击量+1
    this.clickNum();
  },
  methods: {
    //跳转商品详情页面
    gotoDetails (pid) {
      this.$router.push({
        name: 'productDetails',
        query: {
          id: pid
        }
      });
    },
    //跳转商品分类页面
    gotoType (id) {
      this.$router.push({
        name: 'productType',
        query: {
          id: id
        }
      });
    },
    //添加商品进收藏方法
    addLove(id,uid){
      if(this.$store.state.userid !== 0){
        //判断是否是自己的商品
        if(this.$store.state.userid === uid){
          this.$message({
            message: '不能收藏自己发布的商品！',
            type: 'warning'
          });
          return null;
        }
        //判断商品是否添加
        this.$axios.post('/product/selectLoveById', {
          uid: this.$store.state.userid,
          pid: id
        }).then(res => {
          console.log();
          if(res.data < 1){
            //商品添加收藏
            this.$axios.post('/product/insertProductToLove', {
              uid: this.$store.state.userid,
              pid: id
            }).then(res => {
              if(res.data === 1){
                this.$message({
                  message: '商品收藏成功！',
                  type: 'success'
                });
              }
            })
          }else{
            this.$message({
              message: '请勿重复收藏！',
              type: 'warning'
            });
          }
        });
      }else{
        this.$message({
          type: "warning",
          message: "请登录！"
        });
      }
    },
    //统计点击量
    clickNum(){
      //判断数据库中是否存在当前日期
      this.$axios.post('/utils/selectDateFromStatis',{
        dates: moment().format('YYYY-MM-DD')
      }).then(res => {
        if(res.data.length === 0){
          //如果数据库不存在今天的数据，插入今天日期与默认点击量
          this.$axios.post('/utils/insertDateInStatis',{
            dates: moment().format('YYYY-MM-DD'),
          });
        }else{
          //如果数据库存在今天的数据，获取当日点击量+1
          this.$axios.post('/utils/updateNumInStatis',{
            id: res.data.id,
            clickNum: (res.data.clickNum+1)
          });
        }
      });
    },
    gotoLatest(){

      this.$nextTick(() => {
        console.log('233')
        const t =  document.getElementById('latest') 
        if(t){
          t.scrollIntoView({
          behavior: 'smooth',
          block:"start"
      });
        }

      })


    },
    gotoMypage(){
      this.$router.push("/mine")
    }
  }
}
</script>

<style scoped="scoped">
@import url("../css/index.css");


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
