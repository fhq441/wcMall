<template>
<div class="person-wrapper">
    <mt-header title="基本信息">
        <router-link to="" slot="left">
            <mt-button  onclick="window.history.go(-1)" icon="back">返回</mt-button>
        </router-link>
        <mt-button slot="right" @click="save">保存</mt-button>
    </mt-header>
    <div class="content">
        <p>姓名<input type="text" v-model="name"></p>
        <p>生日<input type="text" v-model="age"></p>
        <p>性别<select v-model="sex">
          <option value="男">男</option>
          <option value="女">女</option>
          </select> </p>
        <p>电话<input type="text" v-model="tel"></p>
    </div>
</div>
</template>

<script>
export default {
  data () {
    return {
      name: '',
      age: '',
      sex: '男',
      tel: '',
      sexs: [{'code':'男','name':'男'},{'code':'女','name':'女'}]
    }
  },
  methods: {
    save () {
      if (this.name && this.age && this.age && this.tel !== '') {
        var that = this
        this.show = false
        this.axios.post('http://card.yhy2009.com/frontyuser/add', {
          regidetName: that.name,
          birthday: that.age,
          sex: that.sex,
          regeditPhone: that.tel
        })
        .then(function (r) {
          console.log(r)
          if (r.data.code === 'ok') {
            alert('保存成功')
            // that.$router.replace('/about/setUp')
          } else {
            alert('保存失败')
          }
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
    this.distinguish()
    var that = this
    this.show = false
    this.axios.post('http://card.yhy2009.com/Index/personInfo')
    .then(function (response) {
      that.name = response.data.nickname
      that.age = response.data.age
      that.sex = response.data.sex
      that.tel = response.tel
    })
    .catch(function (error) {
      console.log(error)
    })
  }
}
</script>

<style lang="less">
.person-wrapper{
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    background: #eee;
    .mint-header{
        margin-bottom: 8px;
    }
    .content{
        background: #fff;
        p{
            padding: 0 15%;
            border-top: 1px solid #ddd;
            height: 60px;
            line-height: 85px;
            color: #333;
            input{
                margin-left: 15%;
                border: none;
                width: 70%;
            }
            select{
                margin-left: 15%;
                border: none;
                width: 70%;
            }
        }
    }
}
</style>
