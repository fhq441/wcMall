<template>
<div class="person-wrapper">

   <mt-field label="收件人" placeholder=" 请输入 收件人姓名" type="text" :state="addresseestate" :attr="{ maxlength: 12 }" v-model="addressee"></mt-field>
 <mt-field label="电话" placeholder=" 请输入 收件人电话" v-model="mobile"  type="tel" :state="mobilestate" :attr="{ maxlength: 11 }" ></mt-field>
 <mt-field label="地址" placeholder=" 请输入 收件人地址" v-model="address"  type="text" :state="addressstate" :attr="{ maxlength: 100 }" ></mt-field>
 <mt-field label="券码" placeholder=" 请输入 券码" v-model="codepwd"  type="number" :state="codepwdstate" :attr="{ maxlength: 8 }" ></mt-field> 

 <mt-button   type="default" @click="window.history.go(-1)">返回</mt-button>
  <mt-button size="normal" type="primary"  @click="save">提交</mt-button>
    
</div>
</template>
<script>
 import { Toast } from 'mint-ui';
export default {
  data () {
    return {
      addresseestate: '',
      addressee: '',
      mobile: '',
      codepwd: '',
      address: '',
      mobilestate: '',
      codepwdstate: '',
      addressstate: '',
      pid: '',
      openid: '',
      productDetail: '',
    }
  },
  methods: {
    save () {
      console.log("openid:"+this.openid)
     if(!this.openid){
          Toast({
                message: '请重新授权登录',
                position: 'middle',
                duration: 5000
              });
              return;
     }

      if (this.addressee && this.codepwd && this.address && this.mobile !== '') {
        var that = this
        this.show = false
        that.openid =sessionStorage.openid
        that.changeCode = sessionStorage.changeCode
        this.axios.post('http://card.yhy2009.com/frontyuserOrder/add', {
          addressee: that.addressee,
          codepwd: that.codepwd,
          address: that.address,
          mobile: that.mobile,
          pruoductId: that.pid,
          openid: that.openid,
          exchangeCode: that.changeCode
        })
        .then(function (r) {
          console.log(r)
          if(r.data.status === 'success'){
              Toast({
                message: '兑换成功',
                position: 'middle',
                duration: 5000
              });
             that.$router.push('/about/orderList/delivey')
          }else if(r.data.status === '0001'){
            
             Toast({
                message: '券码号不可用',
                position: 'middle',
                duration: 5000
              });
          }else if(r.data.status === '0002'){
            
             Toast({
                message: '券码不正确',
                position: 'middle',
                duration: 5000
              });
          }
          // if (response.data === 2) {
          //   alert('保存成功')
          //   that.$router.replace('/about/setUp')
          // } else {
          //   alert('保存失败')
          // }
        })
        .catch(function (error) {
          console.log(error)
        })
      } else {
        alert('请填完整')
      }
    }
  },
  mounted: function () {
    this.openid = sessionStorage.openid
    console.log("---------1-----------")
    console.log(this.$route.params.id)
    console.log("--------1------------")
      this.pid = this.$route.params.id
    var that = this
    this.axios.post('http://card.yhy2009.com/frontyproduct/detail/' + this.pid)
    .then(function (r) {
      that.productDetail = r.data
    })
    .catch(function (error) {
      console.log(error)
    })
    // this.distinguish()
    // var that = this
    // this.show = false
    // this.axios.post('http://card.yhy2009.com/Index/personInfo')
    // .then(function (response) {
    //   that.name = response.data.nickname
    //   that.age = response.data.age
    //   that.sex = response.data.sex
    //   that.tel = response.tel
    // })
    // .catch(function (error) {
    //   console.log(error)
    // })
  }
}
</script>

<style lang="less">
.person-wrapper{
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    background: hsl(180, 20%, 96%);
    .mint-header{
        margin-bottom: 8px;
    }
    
    .input{
        width: 60%;
        height: 85%;
        border-radius: 5px;
        margin-left: 10%;
        border: 1px solid rgb(209, 204, 204);
    }
    .input1{
        width: 60%;
        height: 85%;
        border-radius: 5px;
        margin-left: 5%;
        border: 1px solid rgb(209, 204, 204);
    }
    .textarea{
        width: 60%;
        height: 85%;
        border-radius: 5px;
        border: 1px solid rgb(209, 204, 204);
    }
    .content{
        background: #fff;
        p{
            padding: 0 5%;
            border-top: 1px solid #ddd;
            height: 70px;
            line-height: 40px;
            color: #333;
            // input{
            //     margin-left: 10%;
            //     // border: none;
            // }
        }
        .p1{
            padding: 0 5%;
            border-top: 1px solid #ddd;
            height: 90px;
            line-height: 40px;
            color: #333;
            textarea{
                margin-left: 10%;
                // border: none;
            }
        }
    }
    .right{
      float: right;
      width: 100%;
      height: 50px;
      font-size: 0;
      button{
        padding: 0;
        margin: 0;
        border: none;
        height: 50px;  
        font-size: 14px;
        color:#ebebeb;
      }
      .add-cart{
        background: #999;
        width: 45%;
        float: left;
        outline: none;
      }
      .buy-now{
        background: #04ec23;
        width: 55%;
        float: right;
      }
    }
}
</style>
