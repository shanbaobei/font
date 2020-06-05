<template>
  <div id="app">
    <div class="layui-container">
      <form class="layui-form layui-form-pane" action="">
        <div class="layui-form-item" :class="{'form-grop--error':
  $v.name.$error}">
    <label class="layui-form-label">用户名</label>
  <div class="layui-input-inline" >
      <input type="text" name="title" placeholder="请输入标题" autocomplete="off" class="layui-input"
      v-model.trim="$v.name.$model">
    </div>
    <div class="error layui-form-mid" v-if="!$v.name.required">用户名不得为空</div>
    <div class="error layui-form-mid" v-if="!$v.name.email">用户名格式输入错误</div>
  </div>
   <div class="layui-form-item">
    <label class="layui-form-label">密码</label>
    <div class="layui-input-block">
      <input type="password" name="title" required  lay-verify="required" placeholder="请输入标题" autocomplete="off" class="layui-input"
      v-model="password">
    </div>
  </div>
   <div class="layui-form-item">
    <label class="layui-form-label">验证码</label>
    <div class="layui-input-inline">
      <input type="text" name="title" required  lay-verify="required" placeholder="请输入标题" autocomplete="off" class="layui-input"
      v-model="code">
    </div>
    <div class="layui-form-mid svg" v-html="svg" @click="getCaptcha()">图片</div>
  </div>
  <button type="button" class="layui-btn" @click="checkForm">点击登录</button>
<a class="imooc-link" href="http://www.layui.com">忘记密码</a>
 </form>
    </div>
 
  </div>
</template>
<script>
import axios from 'axios'
import { required,email} from 'vuelidate/lib/validators'
export default {
  name:'app',
  data (){
    return {
      svg: '',
      name:'',
      password:'',
      code:'',
      errorMsg:[]

    }
  },
  validations: {
    name: {
      required,
      email
    },
    password: {
      required
    },
    code: {
      required
    }
  },
  mounted () {
    this.getCaptcha()
  },
  methods: {
    getCaptcha (){
      axios.get('http://localhost:3000/getCaptcha').then ((res) => {
      // console.log(res)
      if (res.status ===200){
        let obj = res.data
        if (obj.code === 200){
          this.svg = obj.data
        }
      }
    })
    },
    checkForm() {
      this.errorMsg = []
      if (!this.name) {
        this.errorMsg.push('登录名为空!')
      }
      if (!this.password) {
        this.errorMsg.push('密码为空!')
      }
      if (!this.code) {
        this.errorMsg.push('验证码为空!')
      }
    }

  } 
}
</script>





<style lang="scss" scoped>
#app {
  background: #f2f2f2;
}
.layui-container {
  background:#fff;
}
input {
  width: 190px;
}
.imooc-link {
  margin-left: 10px;
  &:hover {
    color:#009688;
  }
}
.svg {
  position: relative;
  top:-15px
}
.error {
    display: block;
  }
.form-grop--error {
  .error {
    display: block;
  }
}
</style>