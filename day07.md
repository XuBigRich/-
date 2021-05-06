### 今日目标
1.初始化项目 
2.基于Vue技术栈进行项目开发
3.使用Vue的第三方组件进行项目开发
4.理解前后端分离开发模式

### 1.电商业务概述
客户使用的业务服务：PC端，小程序，移动web，移动app
管理员使用的业务服务：PC后台管理端。
PC后台管理端的功能：管理用户账号（登录，退出，用户管理，权限管理），商品管理（商品分类，分类参数，商品信息，订单），数据统计
电商后台管理系统采用前后端分离的开发模式
前端项目是基于Vue的SPA（单页应用程序）项目

前端技术栈:Vue,Vue-Router,Element-UI,Axios,Echarts
后端技术栈：Node.js,Express,Jwt(模拟session),Mysql,Sequelize(操作数据库的框架)

### 2.项目初始化
A.安装Vue脚手架
B.通过脚手架创建项目
C.配置路由
D.配置Element-UI:在插件中安装，搜索vue-cli-plugin-element
E.配置Axios：在依赖中安装,搜索axios(运行依赖)
F.初始化git仓库
G.将本地项目托管到github或者码云中

### 3.码云相关操作
A.注册登录码云账号
![](images/注册码云.jpg)

B.安装git
    在Windows上使用Git，可以从Git官网直接下载安装程序进行安装。
    测试命令：git --version

C.点击网站右上角“登录”，登录码云，并进行账号设置
![](images/码云点击设置.jpg)

D.在本地创建公钥：在终端运行：ssh-keygen -t rsa -C "xxx@xxx.com"
![](images/创建公钥.jpg)
E.找到公钥地址：
Your identification has been saved in /c/Users/My/.ssh/id_rsa.
Your public key has been saved in /c/Users/My/.ssh/id_rsa.pub.
当我们创建公钥完毕之后，请注意打印出来的信息“Your public key has been saved in”
/c/Users/My/.ssh/id_rsa.pub : c盘下面的Users下面的My下面的.ssh下面的id_rsa.pub就是我们创建好的公钥了

E.打开id_rsa.pub文件，复制文件中的所有代码，点击码云中的SSH公钥，将生成的公钥复制到公钥中
![](images/pub文件.jpg)

![](images/ssh公钥.jpg)
G.测试公钥：打开终端，输入命令
ssh -T git@gitee.com
![](images/success.jpg)

H.将本地代码托管到码云中
点击码云右上角的+号->新建仓库
![](images/新建仓库.jpg)
![](images/创建仓库2.jpg)

I.进行git配置：
![](images/config.jpg)

1. 现执行git status 检查本地仓库状态
2. 在执行 git add . 
3. 在执行 git commit -m 'addfile' 提交到本地缓存区

打开项目所在位置的终端，进行git仓库关联
![](images/项目终端执行关联.jpg)













### 4.配置后台项目

A.安装phpStudy并导入mysql数据库数据
![](images/phpStudy.jpg)

![](images/mysql.jpg)
B.安装nodeJS，配置后台项目,从终端打开后台项目vue_api_server
然后在终端中输入命令安装项目依赖包：npm install

C.使用postman测试api接口
![](images/postman.jpg)

### 5.实现登录功能
A.登录状态保持
如果服务器和客户端同源，建议可以使用cookie或者session来保持登录状态
如果客户端和服务器跨域了，建议使用token进行维持登录状态。

B.登录逻辑：
在登录页面输入账号和密码进行登录，将数据发送给服务器
服务器返回登录的结果，登录成功则返回数据中带有token
客户端得到token并进行保存，后续的请求都需要将此token发送给服务器，服务器会验证token以保证用户身份。

**C.添加新分支login，在login分支中开发当前项目vue_shop：**
打开vue_shop终端，使用git status确定当前项目状态。
确定当前工作目录是干净的之后，创建一个分支进行开发，开发完毕之后将其合并到master
git checkout -b login
然后查看新创建的分支：git branch
确定我们正在使用login分支进行开发
![](images/branch.jpg)



然后执行vue ui命令打开ui界面，然后运行serve，运行app查看当前项目效果
![](images/ui界面启动项目.jpg)

发现现在是一个默认页面，我们需要进行更改，打开项目的src目录，点击main.js文件（入口文件）
```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import './plugins/element.js'

Vue.config.productionTip = false


new Vue({
  router,
  render: h => h(App)
}).$mount('#app')

```
再打开App.vue(根组件)，将根组件的内容进行操作梳理(template中留下根节点，script中留下默认导出，去掉组件，style中去掉所有样式)
```
<template>
  <div id="app">
  	app根组件
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
</style>
```
再打开router.js(路由)，将routes数组中的路由规则清除，然后将views删除，将components中的helloworld.vue删除
```
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  routes: [
    
  ]
})
```


在components文件夹中新建Login.vue组件,添加template，script，style标签,style标签中的scoped可以防止组件之间的样式冲突，没有scoped则样式是全局的

```
<template>
    <div class="login_container">
        登录组件
    </div>
</template>

<script>
export default {
  
}
</script>

<style lang="less" scoped>
.login_container {
  background-color: #2b5b6b;
  height: 100%;
}
</style>

```
在router.js中导入组件并设置规则
在App.vue中添加路由占位符

```
import Login from '../components/Login.vue'
const router = new Router({
  routes: [
    { path: '/', redirect: '/login' },
    { path: '/login', component: Login }
  ]
})
```

当我们给Login.vue中的内容添加样式的时候，会报错“缺少less-loader”，需要配置less加载器（开发依赖），安装less(开发依赖)
![](images/less.jpg)
然后需要添加公共样式，在assets文件夹下面添加css文件夹，创建global.css文件,添加全局样式

```
/* 全局样式表 */
html,body,#app{
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0; 
}
```
在main.js中导入global.css，使得全局样式生效 import "./assets/css/global.css"

```
import './assets/css/global.css'
```

然后Login.vue中的根元素也需要设置撑满全屏（height:100%）

在屏幕中央绘制登录盒子

```
<div class="login_container">
    <!-- 登录盒子  -->
    <div class="login_box">

    </div>
</div>

<style lang="less" scoped>

.login_box {
  width: 450px;
  height: 300px;
  background: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  }
</style>
```

绘制默认头像区域

```
 <div class="login_box">
     <!-- 头像 -->
     <div class="avatar_box">
     	<img src="../assets/logo.png" alt="" />
     </div>
</div>

<style lang="less" scoped>
.avatar_box {
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
</style>
```

绘制登录表单区域 

```html
// 因为要使用 element 表单控件 所以要在 plugins 下的 element.js 中引入 form 控件
import { Form } from 'element-ui'
import { FormItem } from 'element-ui'
import { Input } from 'element-ui
Vue.use(Form)
Vue.use(FormItem)
Vue.use(Input)

// DOM 结构 
<div class="login_box">
    <!-- 头像 -->
    ...........
    <!-- 登录表单 -->
    <el-form
             label-width="0px"
             class="login_form"
             >
        <!-- 用户名 -->
        <el-form-item prop="username">
            <el-input></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
            <el-input></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
            <el-button type="primary">登录</el-button>
            <el-button type="info">重置</el-button>
        </el-form-item>
    </el-form>
</div>
    
<style> 
    .login_form {
        position: absolute;
        bottom: 0;
        width: 100%;
        padding: 0 20px;
        box-sizing: border-box;
    }
    .btns {
        display: flex;
        justify-content: flex-end;
    }
</style>
```

绘制带icon 的输入框

```html
# 在assets 中 放入 字体图标文件 
# 在 main.js 中导入全局样式表
import './assets/css/global.css'


# Login 组件中 
<!-- 用户名 -->
<el-form-item>
    <el-input
              prefix-icon="iconfont icon-user"
              ></el-input>
</el-form-item>
<!-- 密码 -->
<el-form-item>
    <el-input
              prefix-icon="iconfont icon-3702mima"
              ></el-input>
</el-form-item>
```

实现表单的数据绑定

```html
# Login 组件中 
<!-- 用户名 -->
 <el-form
          #1. 
        :model="loginForm"
        label-width="0px"
        class="login_form"
      >
<el-form-item>
    <el-input
              #2. v-model="loginForm.username"
              prefix-icon="iconfont icon-user"
              ></el-input>
</el-form-item>
<!-- 密码 -->
<el-form-item>
    <el-input
              #2. v-model="loginForm.password"
              prefix-icon="iconfont icon-3702mima"
              ></el-input>
</el-form-item>

<script>
export default {
  data() {
    return {
      //数据绑定
        #3. 
      loginForm: {
        username: "admin",
        password: "123456",
      },
    }
  }
</script>
```

实现表单的数据验证

```html
# Login 组件中
<el-form
        :model="loginForm"
         #1 给form 绑定校验规则
        :rules="loginFormRules"
        label-width="0px"
        class="login_form"
      >
        <!-- 用户名 -->
        <el-form-item 
                      #2. 给表单元素绑定校验规则
                      prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="iconfont icon-user"
          ></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item 
                      #2. 给表单元素绑定校验规则
                      prop="password">
          <el-input
            type="password"
            v-model="loginForm.password"
            prefix-icon="iconfont icon-3702mima"
          ></el-input>
        </el-form-item>
      </el-form>


<script>
export default {
  data() {
    return {
      loginForm: {},
       #3. //表单验证规则
      loginFormRules: {
        username: [
          { required: true, message: "请输入登录名", trigger: "blur" },
          {
            min: 3,
            max: 10,
            message: "登录名长度在 3 到 10 个字符",
            trigger: "blur",
          },
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          {
            min: 6,
            max: 15,
            message: "密码长度在 6 到 15 个字符",
            trigger: "blur",
          },
        ],
      }
    }
  }
</script>
```

实现表单重置功能

```html
  <!-- 按钮 -->
<el-form
         :model="loginForm"
         #1. 增加 ref属性
         ref="LoginFormRef"
         :rules="loginFormRules"
         label-width="0px"
         class="login_form"
         >
    .....
    <el-form-item class="btns">
        <el-button type="primary">登录</el-button>
        #2. 增加重置方法
        <el-button type="info" @click="resetLoginForm">重置</el-button>
    </el-form-item>
</el-form>

<script>
export default {
  data() {
    return {}
  },
  //添加行为，
  methods: {
    //#3. 添加表单重置方法
    resetLoginForm() {
      //this=>当前组件对象，其中的属性$refs包含了设置的表单ref
      //   console.log(this)
      this.$refs.LoginFormRef.resetFields();
    },
  }
```

登录前表单数据验证

```html
<!-- 按钮 -->
<el-form-item class="btns">
    #1. 绑定登录事件
    <el-button type="primary" @click="login">登录</el-button>
    <el-button type="info" @click="resetLoginForm">重置</el-button>
</el-form-item>

...
methods: {
	 resetLoginForm(){},
	 #2. 设置处理函数
	 login() {
      //点击登录的时候先调用validate方法验证表单内容是否有误
      this.$refs.LoginFormRef.validate( (valid) => {
        console.log(valid);
      });
    },
}
```

配置 axios 发送请求

```html
// 1. 在 main.js 文件中 引入 axios 
import axios from 'axios'
//  配置请求根路径
axios.defaults.baseURL = 'http://127.0.0.1:8888/api/private/v1/'
Vue.prototype.$http = axios

// login.vue
...
methods: {
	 resetLoginForm(){},
	 #2. 设置处理函数
	 login() {
      //点击登录的时候先调用validate方法验证表单内容是否有误
      this.$refs.LoginFormRef.validate(async (valid) => {
        console.log(valid);
        if (!valid) return
		#2. 
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return console.log('登录失败！')
        console.log('登录成功')
    },
}

```

配置全局 message 弹框组件

```html
// 1. 在element.js 中 导入
import { Message } from 'element-ui'
Vue.prototype.$message = Message

#2. 
 methods: {
    // 点击重置按钮，重置登录表单
    resetLoginForm() {
      // console.log(this);
      this.$refs.loginFormRef.resetFields()
    },
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功')
      })
    }
  }
```

登录之后的操作行为

```html
 methods: {
    // 点击重置按钮，重置登录表单
    resetLoginForm() {
      // console.log(this);
      this.$refs.loginFormRef.resetFields()
    },
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功')
        // 1. 将登录成功之后的 token，保存到客户端的 sessionStorage 中
        //   1.1 项目中初了登录之外的其他API接口，必须在登录之后才能访问
        //   1.2 token 只应在当前网站打开期间生效，所以将 token 保存在 sessionStorage 中
		#1. 
        window.sessionStorage.setItem('token', res.data.token)
        // 2. 通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.push('/home')
      })
    }
  }

// 2. 新建 Home.vue 组件
<template>
	<div>
        Home 组件
    </div>
</template>

// 3. 在路由规则中 增加 home 规则
import Home from './components/Home.vue'
const router = new Router({
  routes: [
    { path: '/', redirect: '/login' },
    { path: '/login', component: Login },
	// 增加 home 规则
    { path: '/home', component: Home }
  ]
})
```

路由导航控制页面权限

```html
// 在 router.js 配置导航守卫
// 挂载路由导航守卫
router.beforeEach((to, from, next) => {
  // to 将要访问的路径
  // from 代表从哪个路径跳转而来
  // next 是一个函数，表示放行
  //     next()  放行    next('/login')  强制跳转

  if (to.path === '/login') return next()
  // 获取token
  const tokenStr = window.sessionStorage.getItem('token')
  if (!tokenStr) return next('/login')
  next()
})
```

最终Login.vue文件中的代码如下

```
<template>
    <div class="login_container">
        <!-- 登录盒子  -->
        <div class="login_box">
            <!-- 头像 -->
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="">
            </div>
            <!-- 登录表单 -->
            <el-form :model="loginForm" ref="LoginFormRef" :rules="loginFormRules" label-width="0px" class="login_form">
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user" ></el-input>
                </el-form-item> 
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input type="password" v-model="loginForm.password" prefix-icon="iconfont icon-3702mima"></el-input>
                </el-form-item> 
                <!-- 按钮 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item> 
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      //数据绑定
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      //表单验证规则
      loginFormRules: {
        username: [
          { required: true, message: '请输入登录名', trigger: 'blur' },
          {
            min: 3,
            max: 10,
            message: '登录名长度在 3 到 10 个字符',
            trigger: 'blur'
          }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          {
            min: 6,
            max: 15,
            message: '密码长度在 6 到 15 个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  //添加行为，
  methods: {
    //添加表单重置方法
    resetLoginForm() {
      //this=>当前组件对象，其中的属性$refs包含了设置的表单ref
      //   console.log(this)
      this.$refs.LoginFormRef.resetFields()
    },
    login() {
      //点击登录的时候先调用validate方法验证表单内容是否有误
      this.$refs.LoginFormRef.validate(async valid => {
        console.log(this.loginFormRules)
        //如果valid参数为true则验证通过
        if (!valid) {
          return
        }

        //发送请求进行登录
        const { data: res } = await this.$http.post('login', this.loginForm)
        //   console.log(res);
        if (res.meta.status !== 200) {
          return this.$message.error('登录失败:' + res.meta.msg) //console.log("登录失败:"+res.meta.msg)
        }

        this.$message.success('登录成功')
        console.log(res)
        //保存token
        window.sessionStorage.setItem('token', res.data.token)
        // 导航至/home
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  background-color: #2b5b6b;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  .avatar_box {
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}
.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns {
  display: flex;
  justify-content: flex-end;
}
</style>

```

实现退出功能
在Home组件中添加一个退出功能按钮,给退出按钮添加点击事件，添加事件处理代码如下：

```vue
<template>
    <div>
        this is home
        <el-button type="info" @click="logout"> 退出 </el-button>
    </div>
</template>

<script>
export default {
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    }
  }
}
</script>

<style lang='less' scoped>
</style>
```
### 补充
A.处理ESLint警告
打开脚手架面板，查看警告信息
默认情况下，ESLint和vscode格式化工具有冲突，需要添加配置文件解决冲突。
在项目根目录添加 .prettierrc 文件

```
{
    "semi":false,
    "singleQuote":true
}
```
打开.eslintrc.js文件，禁用对 space-before-function-paren 的检查:
```
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'space-before-function-paren' : 0
  },
```

B.合并按需导入的 element-ui
```
import Vue from 'vue'
import { Button, Form, FormItem, Input, Message } from 'element-ui'

Vue.use(Button)
Vue.use(Form)
Vue.use(FormItem)
Vue.use(Input)
// 进行全局挂载：
Vue.prototype.$message = Message
```

C.将代码提交到码云
新建一个项目终端,输入命令‘git status’查看修改过的与新增的文件内容
将所有文件添加到暂存区：git add .
将所有代码提交到本地仓库：git commit -m "添加登录功能以及/home的基本结构"
查看分支： git branch  发现所有代码都被提交到了login分支
将login分支代码合并到master主分支，先切换到master：git checkout master
在master分支进行代码合并：git merge login
将本地的master推送到远端的码云：git push

推送本地的子分支到码云，先切换到子分支：git checkout 分支名
然后推送到码云：git push -u origin 远端分支名