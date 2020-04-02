<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app clipped>
      <v-list dense>
        <v-list-item
          v-for="item in items"
          :key="item.text"
          :to="item.link"
          link
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>
              {{ item.text }}
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-subheader class="mt-4 grey--text text--darken-1"
          >SUBSCRIPTIONS</v-subheader
        >
        <v-list v-show="loadStatus">
          <v-list-item
            v-for="item in items2"
            :key="item.text"
            :to="item.link"
            link
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-title v-text="item.text" />
          </v-list-item>
        </v-list>

        <v-list-item v-if="loadStatus" class="mt-4">
          <v-list-item-action>
            <v-icon color="grey darken-1">mdi-lock</v-icon>
          </v-list-item-action>
          <v-list-item-title class=" text--darken-1">
            欢迎您,{{ loadStatus.username }}
          </v-list-item-title>
        </v-list-item>

        <v-list-item v-else="" class="mt-4" @click="isShowLoginForm = true">
          <v-list-item-action>
            <v-icon color="darken-1">mdi-send</v-icon>
          </v-list-item-action>
          <v-list-item-title class=" text--darken-1">登录</v-list-item-title>
        </v-list-item>

        <!-- 注册模块 -->
        <v-list-item
          class="mt-4"
          @click="isShowRegisterForm = true"
          v-show="!loadStatus"
        >
          <v-list-item-action>
            <v-icon color="darken-1">mdi-dialpad</v-icon>
          </v-list-item-action>
          <v-list-item-title class=" text--darken-1"
            >未有账号?去注册</v-list-item-title
          >
        </v-list-item>

        <!-- 退出 -->
        <v-list-item @click="loginOut" v-show="loadStatus">
          <v-list-item-action>
            <v-icon color="darken-1">mdi-arrow-up-bold-box-outline</v-icon>
          </v-list-item-action>
          <v-list-item-title class=" text--darken-1">退出</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app clipped-left color="red" dense>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-icon class="mx-4" large>
        mdi-youtube
      </v-icon>
      <v-toolbar-title class="mr-12 align-center">
        <span class="title">LLLin视频网站</span>
      </v-toolbar-title>
      <v-spacer />
      <v-row align="center" style="max-width: 650px">
        <v-text-field
          :append-icon-cb="() => {}"
          placeholder="Search..."
          single-line
          append-icon="mdi-magnify"
          color="white"
          hide-details
          @click:append="searchBtn"
          clearable
        />
      </v-row>
    </v-app-bar>

    <v-content>
      <v-row justify="center" align="center">
        <v-col>
          <nuxt-child />
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <!-- 登录状态提示窗口 -->
          <v-dialog type="error" v-model="LoadError" max-width="350px">
            <v-card class="mx-auto" max-width="400" outlined>
              <v-list-item three-line>
                <v-list-item-content>
                  <v-list-item-title class="pa-3"
                    >用户名或密码错误 请重新登录</v-list-item-title
                  >
                </v-list-item-content>
              </v-list-item>
            </v-card>
          </v-dialog>
        </v-col>
      </v-row>

      <!-- 提示 -->
      <v-tooltip v-model="showTip" top>
        <h3 class="show-tip-message">{{ showTipMessage }}</h3>
      </v-tooltip>
      <!-- 展示登录界面 -->

      <div class="text-center">
        <v-bottom-sheet v-model="isShowLoginForm" inset>
          <v-sheet class="text-center" height="350px">
            <v-btn
              class="mt-6"
              text
              color="error"
              @click="isShowLoginForm = !isShowLoginForm"
              >close</v-btn
            >

            <v-form
              @submit.prevent="login"
              class="pa-4"
              ref="form"
              v-model="valid"
              :lazy-validation="lazy"
            >
              <v-text-field
                v-model="loginModel.username"
                :rules="emailRules"
                label="用户名/邮箱"
                required
              ></v-text-field>

              <v-text-field
                v-model="loginModel.password"
                :rules="passwordRules"
                label="密码"
                autocomplete="new-password"
                type="password"
                required
              ></v-text-field>

              <v-btn
                :disabled="!valid"
                color="success"
                class="mr-4"
                type="submit"
                large
                rounded
              >
                登录
              </v-btn>

              <v-btn color="error" class="mr-4" @click="reset" rounded large>
                重置
              </v-btn>
              <v-row>
                <v-col>
                  <p @click="noAcoToRegister">未有账号?去注册</p>
                </v-col>
              </v-row>
            </v-form>
          </v-sheet>
        </v-bottom-sheet>
      </div>

      <!-- 展示注册界面 -->

      <v-bottom-sheet v-model="isShowRegisterForm" inset>
        <v-sheet class="text-center" height="350px">
          <v-btn
            class="mt-6"
            text
            color="error"
            @click="isShowRegisterForm = !isShowRegisterForm"
            >close</v-btn
          >

          <v-form
            @submit.prevent="register"
            class="pa-4"
            ref="form"
            v-model="valid"
            :lazy-validation="lazy"
          >
            <v-text-field
              v-model="registerModel.username"
              :rules="emailRules"
              label="用户名/邮箱"
              required
            ></v-text-field>

            <v-text-field
              v-model="registerModel.password"
              :rules="passwordRules"
              label="密码"
              autocomplete="new-password"
              type="password"
              required
            ></v-text-field>

            <v-text-field
              v-model="registerModel.confirmPass"
              :rules="[passwordRules2]"
              label="确认密码"
              autocomplete="new-password"
              type="password"
              required
            ></v-text-field>

            <v-btn
              :disabled="!valid"
              color="success"
              class="mr-4"
              type="submit"
              large
              rounded
            >
              注册
            </v-btn>
          </v-form>
        </v-sheet>
      </v-bottom-sheet>

      <!-- 注册成功提示 -->
    </v-content>
  </v-app>
</template>

<script>
// 引入部分icon

export default {
  props: {
    source: String
  },
  data: () => ({
    loginModel: {}, //登录用户和密码
    registerModel: {
      username: '',
      password: '',
      confirmPass: ''
    },
    isShowLoginForm: false, //是否展示登录表单
    isShowRegisterForm: false, //是否展示注册表单
    // isLoad: false, //是否已登录
    LoadError: false,
    showTip: false,
    showTipMessage: '',
    valid: false,
    lazy: false,
    drawer: null,
    emailRules: [
      v => !!v || '用户名/邮箱不能为空',
      v => /.+@.+\..+/.test(v) || '邮箱格式不对'
    ],
    passwordRules: [
      v => !!v || '密码不能为空',
      v => (v && v.length <= 16 && v.length >= 6) || '密码格式不对 至少为6~16位'
    ],

    items: [
      {
        icon: 'mdi-home',
        text: '首页',
        link: '/'
      },
      {
        icon: 'mdi-trending-up',
        text: '热门评论',
        link: '/HotComment'
      },
      {
        icon: 'mdi-youtube-subscription',
        text: '热门视频',
        link: '/HotVideo'
      },
      {
        icon: 'mdi-history',
        text: '观看历史'
      },
      {
        icon: 'mdi-playlist-play',
        text: '视频列表'
      }
    ],
    items2: [
      {
        icon: 'mdi-account',
        text: '个人中心',
        link: '/profile'
      },
      {
        icon: 'mdi-message-text',
        text: '消息中心',
        link: '/HotComment'
      },
      {
        icon: 'mdi-domain',
        text: '积分商城',
        link: '/HotComment'
      }
    ]
  }),
  created() {
    this.$vuetify.theme.dark = true
  },
  mounted() {
    //检验token 因为采用的是ssr模式 不能放在created
    // const token = localStorage.getItem('auth._token.local')
    // if(token){
    //   this.isLoad = true;
    // }
  },
  // watch: {
  //   'registerModel.confirmPass': {
  //     //深度监听，可监听到对象、数组的变化
  //     handler(val, oldVal) {
  //       console.log('b.value: ' + val, oldVal) //但是这两个值打印出来却都是一样的
  //     },
  //     deep: true
  //   }
  // },
  methods: {
    //确认密码的另外一种写法
    passwordRules2(value) {
      if (value.length === 0) {
        return '密码不能为空'
      }
      if (value.length > 16 || value.length < 6) {
        return '密码格式错误 应为6~16位'
      }
      if (value !== this.registerModel.password) {
        return '两次输入的密码不一致 请重新输入'
      } else {
        return true
      }
    },

    searchBtn() {
      console.log('click')
    },

    //登录
    async login() {
      try {
        //调用登录接口
        let response = await this.$auth.loginWith('local', {
          data: this.loginModel
        })
        // this.isLoad = true //修改登录状态
        //关闭表单
        this.isShowLoginForm = false
      } catch (err) {
        this.LoadError = true
      }
    },

    // 退出
    async loginOut() {
      //  localStorage.removeItem('auth._token.local')
      //  localStorage.removeItem('auth.strategy')
      //  localStorage.removeItem('auth._refresh_token.local')
      //  cookie.removeItem('auth._token.local')

      try {
        let response = await this.$auth.logout('local')
      } catch (err) {
        console.log(err)
      }
      //  this.isLoad = false;
    },

    //重置表单
    reset() {
      this.$refs.form.reset()
    },

    //注册
    async register() {
      const res = await this.$axios.$post('auth/register', this.registerModel)
      if (res.stateCodes == '10001') {
        this.showTip = true
        this.showTipMessage = '该用户名/邮箱已被注册 请使用其他账号'
        setTimeout(() => {
          this.showTip = false
        }, 2000)
      } else {
        this.isShowRegisterForm = false
        this.showTip = true
        this.showTipMessage = '注册成功 去登录您的账号吧'
        setTimeout(() => {
          this.showTip = false
        }, 2000)
      }
    },

    //未有账号 去登录
    noAcoToRegister() {
      this.isShowLoginForm = false
      this.isShowRegisterForm = true
    }
  },
  computed: {
    loadStatus() {
      return this.$store.state.auth.user
    }
  }
}
</script>

<style lang="scss">
  .show-tip-message{
    padding: 0 580px;
  }
</style>
