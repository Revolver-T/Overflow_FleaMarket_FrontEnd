<template>
    <div class="containerIndex">
      <Search style="margin-top: 0px;"></Search>
      <HeaderTop></HeaderTop>
      <el-backtop></el-backtop>
      <el-divider v-if="this.newpro!==''"></el-divider>
      <!-- 最新上架-->
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
      <FooterBottom></FooterBottom>
    </div>
  </template>


<script>
import HeaderTop from "../components/Header.vue";
import Search from "../components/Search.vue";
import FooterBottom from "../components/Footer.vue";

export default {
  components: {
    HeaderTop,
    Search,
    FooterBottom
  },
  data () {
    return {
      seen: false,
      current: 0,
      imgwidth: "30px",
      newpro: '',//最新商品
    };
  },
  created() {
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
  }
}
</script>