# [Day 15] Vue-Router實戰以及進階使用

# **實戰部分：**

昨天教大家怎麽去創建一個Vue-Router，今天就來教大家怎麽使用RouterLink和RouterView去將畫面渲染出來。

像如果是只有前端的網站，或是後端的進入點只會有一個的話，我們通常會在App.vue上面去直接放一個RouterView：

```
<template>
  <router-view />
</template>

```

這時候我們的路由對到的網址會是我們的根目錄：`localhost:3000`

這個時候我們只要在router.js裏面接上對應的路由：

```tsx
const routes = [
  {
    path: "/",
    redirect: "/index",
  },
  {
    path: "/index",
    component: () => import("@/layouts/sandwich/SandwichLayout.vue"),
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
];

```

這邊我放了一個`redirect: "/index"`，是因爲我希望我的每一個頁面包括主頁面，雖然現在目前還不會使用到子頁面，但是我希望未來可能要做的時候不用去修改現有的路由，因此才把首頁換成`/index`，然後注意我在component那邊是引進一個`SandwichLayout.vue`：

```xml
<template>
    <v-app>
        <v-layout class="rounded rounded-md">
            <MyHeader />
            <default-view />
        </v-layout>
        <MyFooter />
    </v-app>
</template>

<script setup>
import DefaultView from '../default/View';
import MyFooter from '../header_footer/Footer.vue';
import MyHeader from '../header_footer/Header.vue';
</script>

```

可以看到的是我在這個SandwichLayout裏面，做了一個三層式的，像三明治（所以才取名SandwichLayout）一樣的結構，最上面是一個header，中間是main，下面是footer。

而這個時候我的中間的`default-view`裏面是長這個樣子的：

```xml
<template>
  <v-main class="bg-grey-lighten-3">
    <router-view />
  </v-main>
</template>

```

也就是説，我這邊又放了一個路由，所以會對應到我們在routes裏面的`children`選項：

`component: () =>import(/* webpackChunkName: "home" */ "@/views/Home.vue"),`

這邊才是我顯示在首頁上的代碼：

```scala
<template>
  <V-container fluid class="bg-deep-purple-darken-3 pa-0 fill-height">
    <v-parallax src="../assets/home-background-banner.jpg" class="align-center fill-height">
      <v-container style="max-width: 1280px;">
        <v-row>
          <v-col>
            <v-card color="rgba(0,0,0,0)" class="elevation-15 rounded-xl" style="backdrop-filter: blur(4px)">
              <v-card-title class="text-h2 ma-2" style="line-height: 1.5">Hello World!</v-card-title>
              <v-card-subtitle class="text-h5 ma-2" style="line-height: 1.3">This is an example</v-card-subtitle>
              <v-card-text class="text-h5 ma-2" style="line-height: 1.3">
                Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>

      <Homecard :experiences="experiences"></Homecard>

    </v-parallax>
  </V-container>
</template>

```

### **範例展示圖：**

![https://ithelp.ithome.com.tw/upload/images/20230920/20162542JOpcMmYshq.png](https://ithelp.ithome.com.tw/upload/images/20230920/20162542JOpcMmYshq.png)

# **進階部分：**

1. 路由參數：你可以在路由路徑中定義參數，以便根據不同的參數值加載不同的組件。例如，在以下路由中，:id 是一個動態參數，可以根據不同的 id 值加載不同的用戶資料。
    
    ```
    const routes = [
      {
        path: '/user/:id',
        component: UserProfile
      }
    ];
    
    ```
    
2. 路由導航守衛(Navigation Guards)：顧名思義，Vue 路由器提供的導航保護主要通過重定向或取消導航來保護導航。有多種方法可以掛鉤路由導航過程：全局、每個路由或組件內。
    - router.beforeEach
    - router.beforeResolve
    - router.afterEach
    - beforeEnter
    
    或是你還可以自己添加守衛：
    
    ```coffeescript
    const UserDetails = {
      template: `...`,
      beforeRouteEnter(to, from) {
        // called before the route that renders this component is confirmed.
        // does NOT have access to `this` component instance,
        // because it has not been created yet when this guard is called!
      },
      beforeRouteUpdate(to, from) {
        // called when the route that renders this component has changed, but this component is reused in the new route.
        // For example, given a route with params `/users/:id`, when we navigate between `/users/1` and `/users/2`,
        // the same `UserDetails` component instance will be reused, and this hook will be called when that happens.
        // Because the component is mounted while this happens, the navigation guard has access to `this` component instance.
      },
      beforeRouteLeave(to, from) {
        // called when the route that renders this component is about to be navigated away from.
        // As with `beforeRouteUpdate`, it has access to `this` component instance.
      },
    }
    
    ```
    
3. 路由元(meta)信息:你可以在路由配置中添加元信息，這些元信息可以在導航守衛中使用。這對於頁面級別的自定義數據非常有用。
    
    ```yaml
    const routes = [
      {
        path: '/profile',
        component: UserProfile,
        meta: {
          requiresAuth: true
        }
      }
    ];
    
    ```
    
4. 編程式導航：除了使用`<router-link>`組件來處理導航，你還可以在組件中使用編程式導航，例如使用router.push()、router.replace()和router.go()等方法來導航到不同的路由。
    
    ```less
    // 編程式導航到 /user/1router.push('/user/1');
    
    ```
    

當然很多東西還可以說，但是基本上瞭解這一些對於新手來説就很足夠了，足已完成一個小型的前端架構。如果還有興趣，可以到Vue Router的官網查看官方文檔。

本篇終。