# [Day 23] Vuetify專案練習-登入系統(上)

今天來教大家簡單製作登入系統的前端。可能有的人會有疑問，不就是登入界面和注冊界面嗎？這還需要別人來教？當然，如果只是簡單的登入登出，注冊賬號的系統可以很簡單就只做出來，但是我們可能會遇到用戶忘記密碼的時候，這時候可能需要一些驗證，要發送OTP然後驗證，我們可以做幾個簡單的流程：

首先我們需要一個注冊的界面，入口在appbar上面的登入按鈕：

![https://ithelp.ithome.com.tw/upload/images/20230928/20162542SuHtvN5g4X.png](https://ithelp.ithome.com.tw/upload/images/20230928/20162542SuHtvN5g4X.png)

按下后可以顯示登入和注冊按鈕（這邊暫時只有登入）：

![https://ithelp.ithome.com.tw/upload/images/20230928/20162542sWVVysjM85.png](https://ithelp.ithome.com.tw/upload/images/20230928/20162542sWVVysjM85.png)

```xml
<v-menu min-width="200px" rounded>
  <template v-slot:activator="{ props }">
    <v-btn icon v-bind="props">
      <v-avatar color="brown" size="large">
        <v-icon>mdi-login</v-icon>
      </v-avatar>
    </v-btn>
  </template>
  <v-card>
    <v-card-text>
      <div class="mx-auto text-center">
        <v-btn rounded variant="text" to="/login">
          Login
        </v-btn>
      </div>
    </v-card-text>
  </v-card>
</v-menu>

```

這裏我們用"to"直接route到我們的頁面（router的部分會放在最後面）：

![https://ithelp.ithome.com.tw/upload/images/20230928/20162542ir04LKIYHj.png](https://ithelp.ithome.com.tw/upload/images/20230928/20162542ir04LKIYHj.png)

這裏的登入功能還沒做，我們先來看那個"Forgot login Password"的部分：

```
<a @click.prevent="OTPPage" class="text-caption text-decoration-none text-blue" href="#" rel="noopener noreferrer" target="_blank">Forgot login password?</a>

```

這邊用了click事件的修飾符prevent，用來阻止頁面直接跳轉，我希望它能夠是透過vue router來跳轉頁面：

```
methods: {
    OTPPage() {
        router.push({ name: 'OTP' })
    }
}

```

然後會進到這個頁面：

![https://ithelp.ithome.com.tw/upload/images/20230928/2016254253nXzyPNDX.png](https://ithelp.ithome.com.tw/upload/images/20230928/2016254253nXzyPNDX.png)

只要一進入這個頁面，就會開始倒計時60秒，并且在輸入正確的驗證碼之後，會延遲3秒然後跳轉到輸入新密碼的頁面。

輸入成功的時候：

![https://ithelp.ithome.com.tw/upload/images/20230928/20162542TybZjnJuDQ.png](https://ithelp.ithome.com.tw/upload/images/20230928/20162542TybZjnJuDQ.png)

跳轉過後的頁面：

![https://ithelp.ithome.com.tw/upload/images/20230928/20162542ldUUWSsU3l.png](https://ithelp.ithome.com.tw/upload/images/20230928/20162542ldUUWSsU3l.png)

然後登入過後就可以進到User自己的dashboard裏面啦（我還沒做）。

這邊附上所有的程式碼：

```
//Login.vue

<template>
    <div>
        <v-img class="mx-auto my-6" max-width="228"
            src="https://cdn.vuetifyjs.com/docs/images/logos/vuetify-logo-v3-slim-text-light.svg"></v-img>

        <v-card class="mx-auto pa-12 pb-8" elevation="8" max-width="448" rounded="lg">
            <div class="text-subtitle-1 text-medium-emphasis">Account</div>

            <v-text-field density="compact" placeholder="Email address" prepend-inner-icon="mdi-email-outline"
                variant="outlined"></v-text-field>

            <div class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between">
                Password

                <a @click.prevent="OTPPage" class="text-caption text-decoration-none text-blue" href="#"
                    rel="noopener noreferrer" target="_blank">
                    Forgot login password?</a>
            </div>

            <v-text-field :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'" :type="visible ? 'text' : 'password'"
                density="compact" placeholder="Enter your password" prepend-inner-icon="mdi-lock-outline" variant="outlined"
                @click:append-inner="visible = !visible"></v-text-field>

            <v-card class="mb-12" color="surface-variant" variant="tonal">
                <v-card-text class="text-medium-emphasis text-caption">
                    Warning: After 3 consecutive failed login attempts, you account will be temporarily locked for three
                    hours. If you must login now, you can also click "Forgot login password?" below to reset the login
                    password.
                </v-card-text>
            </v-card>

            <v-btn block class="mb-8" color="blue" size="large" variant="tonal">
                Log In
            </v-btn>

            <v-card-text class="text-center">
                <a class="text-blue text-decoration-none" href="#" rel="noopener noreferrer" target="_blank">
                    Sign up now <v-icon icon="mdi-chevron-right"></v-icon>
                </a>
            </v-card-text>
        </v-card>
    </div>
</template>
<script>
import router from '@/router';

export default {
    data: () => ({
        visible: false,
    }),
    methods: {
        OTPPage() {
            router.push({ name: 'OTP' })
        }
    }
}
</script>

```

```xml
//Forgotpassword.vue

<template>
    <v-container class="fill-height bg-deep-purple pa-12" fluid>
        <v-card class="ma-auto px-6 py-8 w-100" max-width="512">
            <v-form v-model="form" @submit.prevent="onSubmit">
                <v-text-field v-model="email" :readonly="loading" :rules="required" class="mb-2" clearable
                    label="Email"></v-text-field>
                <br>

                <v-btn :disabled="!form" :loading="loading" block color="success" size="large" type="submit"
                    variant="elevated">
                    Sent OTP
                </v-btn>
            </v-form>
        </v-card>
    </v-container>
</template>

<script>
import router from '@/router'

export default {
    data: () => ({
        form: false,
        email: null,
        loading: false,
    }),

    methods: {
        onSubmit() {
            if (!this.form) return

            this.loading = true

            setTimeout(() => {
                this.loading = false
                router.push({ name: 'OTP', params: { email: this.email } })
            }, 2000)
        },
    },
    computed: {
        required() {
            return [
                (v) => !!v || "Email必須填寫",
                (v) => /.+@.+\..+/.test(v) || "請輸入有效地址",
            ];
        },
    }
}
</script>

```

```
//OTP.vue

<template>
    <v-container class="d-flex flex-column fill-height justify-center">
        <span class="text-h4"><strong>請輸入驗證碼</strong></span>
        <v-otp-input v-model="otp" type="password" :loading="loading" @finish="onFinish"></v-otp-input>
        <v-container class="d-flex flex-row justify-center align-center">
            <div>驗證碼將在{{ time }}秒后失效</div>
            <v-btn class="mx-5 bg-success" text="重新發送" @click="resendOTP"></v-btn>
        </v-container>
        <v-snackbar v-model="snackbar" :color="snackbarColor" :timeout="2000">
            {{ text }}
        </v-snackbar>
    </v-container>
</template>

<script>
import router from '@/router'
export default {
    data: () => ({
        timer: null,
        time: 60,
        loading: false,
        snackbar: false,
        snackbarColor: 'default',
        otp: '',
        text: '',
        expectedOtp: '123456',
        username: 'DogCom',
    }),
    mounted() {
        this.timer = setInterval(this.countdown, 1000);
    },
    methods: {
        async onFinish(rsp) {
            this.loading = true

            setTimeout(() => {
                this.loading = false
                this.snackbarColor = (rsp === this.expectedOtp) ? 'success' : 'warning'
                this.text = `Processed OTP with "${rsp}" (${this.snackbarColor})`
                this.snackbar = true
            }, 1000)

            if (rsp === this.expectedOtp) {
                await new Promise(resolve => setTimeout(resolve, 3000));
                router.push({ name: 'ResetPassword', params: { username: this.username } });

            }
        },
        countdown() {
            this.time--;
            if (this.time == 0) {
                clearInterval(this.timer)
            }
        },
        resendOTP() {
// 像後端發送請求// 後端接收后發送email或短信
        }
    },
    beforeDestroy() {
        clearInterval(this.timer);
    }
}
</script>

```

```xml
//ResetPassword.vue

<template>
    <v-container class="fill-height bg-deep-purple pa-12" fluid>
        <v-card class="ma-auto px-6 py-8 w-100" max-width="512">
            <v-form v-model="form" @submit.prevent="onSubmit">
                <v-text-field type="password" v-model="password" :readonly="loading" :rules="passwordRules" class="mb-2"
                    clearable label="New Password" placeholder="Enter your password"></v-text-field>

                <v-text-field type="password" v-model="confirmPassword" :readonly="loading" :rules="confirmPasswordRules"
                    clearable label="Confirm Password" placeholder="Confirm password"></v-text-field>

                <br>

                <v-btn :disabled="!form" :loading="loading" block color="success" size="large" type="submit"
                    variant="elevated">
                    Reset Password
                </v-btn>
            </v-form>
        </v-card>
    </v-container>
</template>

<script>
import router from '@/router'

export default {
    data: () => ({
        form: false,
        password: null,
        confirmPassword: null,
        loading: false,
    }),

    methods: {
        async onSubmit() {
            if (!this.form) return

            this.loading = true

            setTimeout(() => {
                this.loading = false
            }, 2000)

            await new Promise(resolve => setTimeout(resolve, 3000))
            router.push({ name: 'Dashboard', params: { username: this.username, password: this.password } })
        },
        required(v) {
            return !!v || 'Field is required'
        },
    },

    computed: {
        passwordRules() {
            return [
                (v) => !!v || '密碼必須填寫',
                (v) => (v && v.length >= 8) || '密碼必須至少包含8個字符',
                (v) => /[A-Z]/.test(v) || '密碼必須包含至少一個大寫字母',
                (v) => /[a-z]/.test(v) || '密碼必須包含至少一個小寫字母',
                (v) => /\d/.test(v) || '密碼必須包含至少一個數字',
                (v) => /[@#$%^&+=]/.test(v) || '密碼必須包含至少一個特殊字符 (@#$%^&+=)',
            ];
        },
        confirmPasswordRules() {
            return [
                (v) => !!v || "確認密碼必須填寫",
                (v) => v === this.password || "確認密碼與密碼不匹配",
            ];
        },
    },

}
</script>

```

```tsx
//Router index.js// Composablesimport { createRouter, createWebHashHistory } from "vue-router";

const routes = [
  {
    path: "/",
    redirect: "/index",
  },
  {
    path: "/index",
    component: () => import("@/layouts/default/Default.vue"),
    children: [
      {
        path: "",
        name: "Home",
// route level code-splitting// this generates a separate chunk (about.[hash].js) for this route// which is lazy-loaded when the route is visited.
        component: () =>
          import(/* webpackChunkName: "home" */ "@/views/Home.vue"),
      },
    ],
  },
  {
    path: "/shop",
    component: () => import("@/layouts/default/Default.vue"),
    children: [
      {
        path: "",
        name: "Shop",
        component: () =>
          import(/* webpackChunkName: "shop" */ "@/views/Shop.vue"),
      },
    ],
  },
  {
    path: "/about",
    component: () => import("@/layouts/default/Default.vue"),
    children: [
      {
        path: "",
        name: "About",
        component: () =>
          import(/* webpackChunkName: "about" */ "@/views/About.vue"),
      },
    ],
  },
  {
    path: "/contact",
    component: () => import("@/layouts/default/Default.vue"),
    children: [
      {
        path: "",
        name: "Contact",
        component: () =>
          import(/* webpackChunkName: "shop" */ "@/views/Contact.vue"),
      },
    ],
  },
  {
    path: "/login",
    component: () => import("@/layouts/default/Default.vue"),
    children: [
      {
        path: "",
        name: "Login",
        component: () =>
          import(/* webpackChunkName: "login" */ "@/views/Login.vue"),
      },
      {
        path: "forgotPassword",
        name: "ForgotPassword",
        component: () =>
          import(
/* webpackChunkName: "forgotPassword" */ "@/views/ForgotPassword.vue"
          ),
      },
      {
        path: "otp",
        name: "OTP",
        component: () =>
          import(/* webpackChunkName: "otp" */ "@/views/OTP.vue"),
      },
      {
        path: "resetPassword",
        name: "ResetPassword",
        component: () =>
          import(
/* webpackChunkName: "resetPassword" */ "@/views/ResetPassword.vue"
          ),
      },
    ],
  },
];

const router = createRouter({
  history: createWebHashHistory(process.env.BASE_URL),
  routes,
});

export default router;

```

今天就簡單教大家製作一個簡易的登入系統~因爲還沒有後端，所以暫時都是用setTimeOut的方式去假裝等待接收後端的資料。我們明天再將這個系統完善。

本篇終。