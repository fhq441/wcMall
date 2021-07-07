<template>
<div class="delivery animated fadeIn">
    <div class="conatiner">
        <li v-for="(item, key) in goods" :key="item.id">
            <div class="goods-wrapper">
                <div class="goods">
                    <div class="image">
                        <img :src='item.top_image'>
                    </div>
                    <div class="info-wrapper">
                        <div class="amount-wrapper">
                            <div class="left">
                                <p>{{ item.product_name }}</p>
                                <!-- <p>数量:<span>1</span></p> -->
                                 <p>韵达快递</p>
                            </div>
                            <div class="right">
                                <p>金额：<span class="price">￥{{ item.product_price }}</span></p>
                                <p>fffFFFF12345678</p>
                            </div>
                            
                        </div>
                    </div>
                    
                </div>
            </div>
        </li>
    </div>
    <div class="alert" v-show="show"  @click="hideAlert">
        <p>提醒成功！</p>
    </div>
</div>
</template>
<script>
export default {
  data () {
    return {
      checked: false,
      num1: 1,
      show: false,
      goods: []
    }
  },
  methods: {
    showAlert () {
      this.show = true
    },
    hideAlert () {
      this.show = false
    }
  },
  mounted: function () {
    var that = this
    this.axios.post('http://card.yhy2009.com/frontyuserOrder/list', {
      openid: sessionStorage.openid
    })
    .then(function (r) { 
        that.goods = r.data.rows
    })
    .catch(function (error) {
      console.log(error)
    })
  }
}
</script>
<style lang="less">
p{
    margin: 0;
    padding: 0
}
.el-button{
    background: #fff;
    color: #666;
    border: 1px solid #ddd;
}
li{
    list-style: none;
}
.delivery{
    height: 100%;
    overflow-y: scroll;
    &::-webkit-scrollbar{
        display: none;
    }
    .alert{
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        background: rgba(0, 0, 0, 0.5);
        p{
            position: absolute;
            height: 60px;
            width: 40%;
            top: 45%;
            left: 30%;
            background: #fff;
            text-align: center;
            line-height: 60px;            
        }
    }
    .conatiner{
        position: absolute;
        width: 100%;
        top: 88px;
        bottom: 0px;
        overflow-y: scroll;
        background: #f2f2f2;
        &::-webkit-scrollbar{
            display: none;
        }
        .goods-wrapper{
            background: #fff;
            height: 120px;
            margin-bottom: 8px;
            padding: 1px;
            .goods{
                margin: 20px auto;
                height: 80px;
                margin-left: 5%;
                .image{
                    width: 28%;
                    float: left;
                    height: 80px;
                    img{
                        height: 80px;
                        width: 80px;
                        display: block;
                    }
                }
                .info-wrapper{
                    width: 68%;
                    float: left;
                    margin-left: 3%;
                    height: 80px;
                    color:#333;
                    .description{
                        color:#333;
                    }
                    .amount-wrapper{
                        height: 70px;
                        margin-top: 5px;
                        p{
                            line-height: 25px;
                        }
                        .left{
                            width: 50%;
                            float: left;
                            p{
                                line-height: 35px;
                            }
                        }
                        .right{
                            height: 80px;
                            width: 50%;
                            float: left;
                            text-align: center;
                            p{
                                line-height: 35px;
                            }
                            .price{
                                color:red;
                            }
                            button{
                                margin: 0 auto;
                                width: 80px;
                                height: 25px;
                                display: block;
                                margin-top: 10px;
                                border-radius: 8px;
                                outline: none;
                            }
                        }
                    }
                }            
            }
        }        
    }
}
</style>
