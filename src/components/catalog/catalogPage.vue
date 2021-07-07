<template>

<div>
<mt-loadmore :top-method="loadTop" :bottom-method="loadBottom" :bottom-all-loaded="allLoaded" ref="loadmore">
  <ul>
    <mt-cell v-for="(item, index) in goods" @click="goDetail(item)" :key="item.id"  :title="item.productName">
  <span>￥{{item.productPrice}}</span>
  <span>{{item.prodCity}}</span>
  <img slot="icon" :src="item.topImage" width="100" height="100">
  <div>
    </br>
  </div>
</mt-cell>
  </ul>
</mt-loadmore>



 

</div>
 
</template>

<script>
import { Loadmore } from 'mint-ui';
 import { Toast } from 'mint-ui';
export default {
  data () {
    return {
      recommend: [{
        valuee: ''
      }],
      goods: [],
      activeName: '',
      firstTid: '',
      loading: false,
      allLoaded:false
    }
  },
  methods: {
    loadTop() { 
  this.$refs.loadmore.onTopLoaded();
},
loadBottom() { 
  this.allLoaded = true;// if all data are loaded
  this.$refs.loadmore.onBottomLoaded();
},
 
    changItem (item, key) {
      this.activeName = item.keyy
      console.log(this.activeName)
      this.goods = []
      var that = this
      this.axios.post('http://card.yhy2009.com/frontyproduct/frontlist', {
        productType: that.activeName
      })
      .then(function (r) {
        that.goods = r.data.rows
      })
      .catch(function (error) {
        console.log(error)
      })
    },
    goDetail (item) {
      sessionStorage.id = item.id
      this.$router.push('/catalog/detail/'+item.id)

    }
  },
  mounted: function () {
  //  sessionStorage.openid = 'oTudI6BTjcmyc_EhV515KokQMQe8'
   
  //  this.$toast.show({
  //    text: sessionStorage.openid,
  //    position: 'bottom'
  //  })
  console.log("-------------------------------------")
   console.log(sessionStorage.openid)
   console.log("-------------------------------------")
    var that = this
    // 获取分类
    this.axios.post('http://card.yhy2009.com/frontdict/queryList', {
      tableName: 'y_product',
      fieldName: 'product_type'
    })
    .then(function (response) {
      that.loading = false
      that.recommend = response.data
    })
    .catch(function (error) {
      console.log(error)
    })
    if (sessionStorage.openid === ',,') {
      alert('身份过期，请重新登录！')
    }
    // 获取商品
    // this.axios.post('http://card.yhy2009.com/frontyproduct/frontlist', {
    //   productType: that.activeName
    // })
    let changeCode = sessionStorage.changeCode  
this.axios.post('http://card.yhy2009.com/frontyproduct/queryListByIds/'+changeCode )
    .then(function (r) {
      that.goods = r.data
    })
    .catch(function (error) {
      console.log(error)
    })
  }
}
</script>
<style lang='less'>
@import '../../common/animate.css';
*{
  margin:0;
  padding: 0;
}
.itemActive{
  border-left: 3px solid rgb(250,140,0);
  background: #f4f4f4;
  color: rgb(250,140,0);
}
.product_ul{
  li{
    list-style: none;
    background: #fff;
    width: 30%;
    float: left;
    margin-left: 3%;
    margin-top: 10px;
    img{
      height: 65px;
      width: 100%;
      display: block;
    }
    p{
      font-size: 14px;
      text-align: center;
      color: #333;
      height: 30px;
      line-height: 30px;
      white-space:nowrap;
      overflow:hidden;
      text-overflow:ellipsis;
      padding: 0 2px;
      background: rgba(0,0,0,0.1)
    }
  }
}
.catalog{
  .loadGoods{
    color: red;
    z-index: 2;
    position: fixed;
  }
  .left{
    .leftItem{
        position: fixed;
        top: 0;
        bottom: 50px;
        left: 0;
        width: 25%;
        overflow-y: scroll;
        background: #fff;
      &::-webkit-scrollbar {
        display: none;
      }
      li{
        list-style: none;
        font-size: 0.8rem;
        text-align: center;
        height: 50px;
        line-height: 50px;
        border-bottom: 1px solid #ebebeb;
      }
    }
  }
  .right{
    .rightItem{
        background: #eee; 
        position: fixed;
        top: 0;
        bottom: 50px;
        right: 0;
        width: 75%;
        overflow-y: scroll;
        border-right: 1px solid #ddd;
        .loadGoods{
          width: 100%;
          height: 100%;
          .spinner {
            position: absolute;
            top: 40%;
            left: 20%;
            width: 50px;
            height: 60px;
            text-align: center;
            font-size: 10px;
          }
          .spinner > div {
            background-color: #f29600;
            height: 100%;
            width: 6px;
            display: inline-block;
            
            -webkit-animation: stretchdelay 1.2s infinite ease-in-out;
            animation: stretchdelay 1.2s infinite ease-in-out;
          }
          .spinner .rect2 {
            -webkit-animation-delay: -1.1s;
            animation-delay: -1.1s;
          }
          .spinner .rect3 {
            -webkit-animation-delay: -1.0s;
            animation-delay: -1.0s;
          }
          .spinner .rect4 {
            -webkit-animation-delay: -0.9s;
            animation-delay: -0.9s;
          }
          .spinner .rect5 {
            -webkit-animation-delay: -0.8s;
            animation-delay: -0.8s;
          }
          @-webkit-keyframes stretchdelay {
            0%, 40%, 100% { -webkit-transform: scaleY(0.4) } 
            20% { -webkit-transform: scaleY(1.0) }
          }
          @keyframes stretchdelay {
            0%, 40%, 100% {
              transform: scaleY(0.4);
              -webkit-transform: scaleY(0.4);
            }  20% {
              transform: scaleY(1.0);
              -webkit-transform: scaleY(1.0);
            }
          }
        }
        ul{
          height: auto;
          .product_ul;
          clear: both;
        }
        &::-webkit-scrollbar {
          display: none;
        }
      .title{
        clear: both;
        color: rgb(250,140,0);
      }
    }
  }
}
</style>
