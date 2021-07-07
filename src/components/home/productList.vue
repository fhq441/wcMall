<template>
  <div >
    <mt-cell v-for="(item,key) in moreproductList" :id="item" :key="item.id">
  <span>icon 是图片</span>
  <img slot="icon" src="./banner.png" width="24" height="24">
</mt-cell>

  </div>
</template>
 
<script>
import { Button } from 'mint-ui';
import { Cell } from 'mint-ui';
export default { 
  data () {
    return {
      goods: [],
      banners: [],
      loading: false,
      appid: 'wxf4fcbe12be02841e',
      appSecret: 'c4a989610584527a4f8375b48e5f198c',
      access_token: '',
      expires_in: 0,
      refresh_token: '',
      scope: '',
      openid: '',
      changeCode: '',
      moreproduct: false,
      moreproductList: []

    }
  },
  methods: {

    queryProudct(){
      let changeCode = this.changeCode
      let that = this
this.axios.get('http://card.yhy2009.com/frontychangeCode/queryList/'+changeCode )
.then(function(r){  
  if(JSON.parse(JSON.stringify(r.data)).length ==1){
    let item = r.data[0]
      sessionStorage.id = item.pid
      that.$router.push('/catalog/detail/'+item.pid)
  }else{

    console.log("---------------------"+r.data.size)
    console.log(JSON.stringify(r.data))
  }

})
    },
    banner (item) {
      // sessionStorage.id = item.id
      this.$router.push('/catalog/detail/' + item.id)
    },
    goDetail (item) {
      sessionStorage.id = item.id
      this.$router.push('/catalog/detail/' + item.id)
    },
    wxLogin(){
    window.location.href = "https://open.weixin.qq.com/connect/oauth2/authorize?appid="+this.appid+"&redirect_uri=http://cardmp.yhy2009.com&response_type=code&scope=snsapi_userinfo&state=1234#wechat_redirect"
    },
      getParamters(str){
      var url=window.location.href.split("?")[1];
      if(!url){
        return null;
      }
      console.log("url:"+url);
      var arr1 = url.split("&");
      console.log("arr1:"+arr1);
      for(var i in arr1){
        var arr2=arr1[i].split("=");
        if(str == arr2[0]){
          return arr2[1];
        }
      }
      return null;
      },
  },
  mounted: function () { 
    let code=this.getParamters("code");
    console.log("======code:"+code);
let that =this
this.axios.get('http://card.yhy2009.com/check/snsapiUserinfo?appid='+this.appid+'&secret='+this.appSecret+'&code='+code+'&grant_type=authorization_code')
.then(function(r){
  console.log("-----------3---------")
  console.log(JSON.stringify(r.data))
   console.log(JSON.stringify(r.data.openid))
   console.log("----------2----------")
  if(r.data.openid){
     console.log("-------1-------------")
    sessionStorage.openid = r.data.openid
     console.log("-------4-------------")
    that.$router.push('/catalog')
  }

})
  }
}
</script>

<style lang='less'>
body{
  margin: 0;
} 
li{
 list-style: none;
}
p{
  padding: 0;
  margin: 0;
}
.homePage {
  margin-bottom: 60px;
  position: relative;
  overflow-y: scroll;
  &::-webkit-scrollbar{
    display: none;
  }
  .list{
    width: 100%;
    .loadhome{
      .spinner {
        margin: 100px auto;
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
    .title{
      text-align: center;
      margin-top: 3%;
      font{
        padding-bottom: 2px;
        border-bottom: 2px solid #f29600;
      }
    }
    .goods{
      float: left;
      width: 44%;
      background: #ddd;
      margin-left: 4%;
      margin-top: 4%;
      height: 150px;
      position: relative;
      overflow: hidden;
      img{
        width: 100%;
        height: 150px;
      }
      img[lazy=loading] {
        width: 40;
        height: 40px;
        margin: 55px auto;
      }
      .text{
        width: 100%;
        background: rgba(0, 0, 0, 0.6);
        position: absolute;
        bottom: 0;
        height: 30px;
        line-height: 30px;
        padding: 0 5%;
        color:#999;
        .name{
          width: 60%;
          display: block;
          float: left;
          white-space:nowrap;
          overflow:hidden;
          text-overflow:ellipsis;
        }
        .price{
          color: #f29600;
        }
      }
    }
  }
  .search-top {
    background: #eee;
    height: 50px;
    .el-icon-arrow-left {
      line-height: 50px;
      margin-left: 2%;
    }
    .search {
      height: 30px;
      width: 80%;
      vertical-align: top;
      margin-top: 10px;
      margin-left: 10%;
      border-radius: 8px;
      display: inline-block;
      overflow: hidden;
      font-size: 0;
      background: #fff;
      .el-icon-search {
        margin: 0 2%;
        height: 30px;
        line-height: 30px;
        width: 6%;
        color: #f29600;
        font-size: 15px;
      }
      input {
        height: 30px;
        vertical-align: top;
        outline: none;
        border: none;
        width: 75%;
      }
      .btn-search {
        border: none;
        background: #f29600;
        color: #fff;
        height: 30px;
        line-height: 30px;
        width: 15%;
        vertical-align: top;
        margin: 0;
        padding: 0;
      }
    }
  }
  .top{
        position:absolute;
        height: 30px;
        top: 50px;
        z-index: 1;
        width: 80%;
        left: 10%;
         p{
            margin: 0;
            padding: 0;
            line-height: 30px;
            color: #bbb;
        }
        .location,.name,.search{
            float: left;
            width: 33.3%;
            height: 30px;
        }
        .location{
            text-align: left;
            img{
                width: 12px;
                margin-right: 4%;
                float: left;
                margin-top: 6px;
            }
            .city{
                margin: 0;
                padding: 0;
                float:left;
                line-height: 30px;
            }
            .el-icon-caret-bottom{
                float: left;
                margin-left: 4%;
                font-size: 12px;
                margin-top: 10px;
                color: #bbb;
            }
        }
    }  
}

.mint-swipe{
  height: 175px;
  width: 100%;
  img{
    display: inline-block;
    height: auto;
    width: 100%;
  }
  .mint-swipe-indicator{
    opacity: 1;
    background: #fff;
  }
  .mint-swipe-indicator.is-active{
    background: #f29600;
  }
}

</style>
