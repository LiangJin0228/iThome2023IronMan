# [Day 4] Vue 3 / Vuetify 的專案目錄結構（下）以及整個Vue的完整架構流程

繼上一篇

# **（子層）src/ App.vue & main.js**

這兩個今天就擺在一起講。

`main.js`是Vue.js應用程序的入口文件，它執行以下關鍵任務：

- 導入Vue.js庫和應用程序所需的其他庫和組件。
- 創建Vue實例，並將其掛載到HTML文檔中的一個DOM元素上，通常是一個div元素。
- 配置Vue實例的選項，例如路由器（Vue Router）、狀態管理器（Vuex）以及其他插件。
- 最後，啟動Vue應用程序。

App.vue是Vue.js應用程序的根組件，它包含了應用程序的整體結構和布局。一個Vue.js應用程序通常由多個組件組成，但App.vue是所有組件的容器。這個文件使用了一種稱為單文件組件（SFC）的方式來定義Vue組件的結構、樣式和行為。

是不是看了但是又好像沒有看懂？沒關係沒關係，接下來聽我娓娓道來，我們先看一開始創建專案的時候它幫我們自動生成的文件，我們先來看一個文件叫做index.html：

```xml
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <link rel="icon" href="/favicon.ico" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Welcome to Vuetify 3</title>
</head>

<body>
  <div id="app"></div>
  <script type="module" src="/src/main.js"></script>
</body>

</html>

```

如果對網頁有一點瞭解的話，就會知道這個index.html其實就是我們不管用不用框架，説是用什麽語言都好，我們就喜歡用index.html來當作我們的主頁，也是網站的進入口。所以當我們進來我們的這個專案的時候（比如説，在瀏覽器輸入網址按下enter），就會被導到我們的index.html。我們可以看到它的`<body>`裏面有兩個東西：

```xml
<div id="app"></div>
<script type="module" src="/src/main.js"></script>

```

也就是說在進入的這個首頁，其實裏面什麽内容也沒有，就只給了一個div，取名叫做app，然後下方的script引進了一個JavaScript文件，所以這時候程序跑到這邊來，會進入這個main.js的檔案裏面：

```tsx
/**
 * main.js
 *
 * Bootstraps Vuetify and other plugins then mounts the App`
 */// Componentsimport App from './App.vue'

// Composablesimport { createApp } from 'vue'

// Pluginsimport { registerPlugins } from '@/plugins'

const app = createApp(App)

registerPlugins(app)

app.mount('#app')

```

我們研究研究這個main.js（看不懂沒關係）

它裏面其實就是import了一些東西進來，然後這句`const app = createApp(App)`就是説，我把上面import進來的東西（`import App from './App.vue'`），將它create成一個我們所謂的Vue實體，之後這一句`registerPlugins(app)`就是我們上一篇説到的，給一個Vue實體加載外挂的function。而這一句`app.mount('#app')`,就是説我將剛剛的那個實體，在挂載了一些外挂后，將它綁定到html文件上的`<div id="app"></div>`這個區塊上。

看起來很複雜？沒事，我們看個圖：

![https://ithelp.ithome.com.tw/upload/images/20230909/20162542VfUK0bR72g.png](https://ithelp.ithome.com.tw/upload/images/20230909/20162542VfUK0bR72g.png)

是不是看了流程圖感覺比較清楚了，可是這就會有個疑問，明明html文件裏什麽都沒有，爲什麽會有畫面顯示出來呢？

原來，我們將App.vue這個東西挂上去html的id='app'的div以後，就可以把App.vue的東西顯示在我們的html的畫面上了。

以下是我們的App.vue裏面的程式碼：

```xml
<template>
  <router-view />
</template>

<script setup>
//
</script>

```

它裏面的router-view這個標簽呢，就是我們在插件裏面注冊到的官方的一個插件，這個router是什麽意思呢？讓我贏一個例子來告訴你：

想象一下你住在一個大城市中的一棟公寓樓里，而你的朋友住在另一棟樓。你想去拜訪你的朋友，但你不知道如何到達他的公寓樓。這時，你需要一個導航系統來幫助你找到正確的路線。

- 你的家：這就是你的電腦或手機（當前位置），你有一些信息需要發送給你的朋友。
- 你的朋友的家：這就是你朋友的電腦或手機，它需要接收你發送的信息。
- 道路和交通規則：這就是網絡中的數據傳輸路徑和規則。在計算機網絡中，這些規則決定了數據如何從一個地方傳輸到另一個地方（你把它想象成URL）。
- 導航系統：這就是路由器。路由器就像一個智能的導航系統，它知道城市中的不同道路和公寓樓的位置。當你告訴導航系統你要去哪里，它會幫助你找到最快、最有效的路線，以便你能夠到達目的地。
- 目的地地址：這就像是你朋友公寓樓的地址。你需要告訴導航系統你朋友住在哪里，以便它知道該去哪個地方（把它想象成是你要訪問的某個頁面的文件）。

所以，路由就是像導航系統一樣的東西，它幫助你的數據包找到正確的路徑，以便從你的電腦發送到你朋友的電腦，就像你找到朋友的公寓一樣。這樣，你的信息可以在網絡上順利傳遞到目的地。

我們會透過使用`router-link`和`router-view`來搭配使用，像是我們一開始的進入點App.vue，裏面就設置了一個router-view標簽，我們可以透過查看文件夾src/router/index.js查看我們的路由配置：

```tsx
// Composablesimport { createRouter, createWebHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    component: () => import('@/layouts/default/Default.vue'),
    children: [
      {
        path: '',
        name: 'Home',
// route level code-splitting// this generates a separate chunk (about.[hash].js) for this route// which is lazy-loaded when the route is visited.
        component: () => import(/* webpackChunkName: "home" */ '@/views/Home.vue'),
      },
    ],
  },
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
})

export default router

```

從一開始的文件就可以看到，當我們訪問根節點的時候，就會去到一個component `'@/layouts/default/Default.vue'`，在這個component裏面長這個樣子：

```xml
<template>
  <v-app>
    <default-bar />

    <default-view />
  </v-app>
</template>

<script setup>
  import DefaultBar from './AppBar.vue'
  import DefaultView from './View.vue'
</script>

```

在這個Default.vue裏面，又可以看到他引進了兩個component，一個叫做AppBar.vue，一個叫做View.vue。

AppBar.vue好理解，就是一個header或navbar的概念，那個View.vue是什麽東西呢？我們去看一下它裏面：

```xml
<template>
  <v-main>
    <router-view />
  </v-main>
</template>

<script setup>
//
</script>

```

這邊可以看到又多了一個`<router-view />`的標簽，我們這時又要回去再看一次在文件夾src/router/index.js裏面的内容：

```tsx
// Composablesimport { createRouter, createWebHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    component: () => import('@/layouts/default/Default.vue'),
    children: [
      {
        path: '',
        name: 'Home',
// route level code-splitting// this generates a separate chunk (about.[hash].js) for this route// which is lazy-loaded when the route is visited.
        component: () => import(/* webpackChunkName: "home" */ '@/views/Home.vue'),
      },
    ],
  },
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
})

export default router

```

我們這時候已經是從第一個Default.vue進來了，可是進來以後又有一次路由，所以我們看他們的children的部分，可以看到，這邊會引導我們進入到`component: () => import(/* webpackChunkName: "home" */ '@/views/Home.vue')`這邊。

我們去看一下Home.vue這個文件裏面：

```xml
<template>
  <HelloWorld />
</template>

<script setup>
  import HelloWorld from '@/components/HelloWorld.vue'
</script>

```

這邊又引進了一個component HelloWorld.vue，我們再去看一下它裏面：

```xml
<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <v-img height="300" src="@/assets/logo.svg" />

      <div class="text-body-2 font-weight-light mb-n1">Welcome to</div>

      <h1 class="text-h2 font-weight-bold">Vuetify</h1>

      <div class="py-14" />

      <v-row class="d-flex align-center justify-center">
        <v-col cols="auto">
          <v-btn
            href="https://vuetifyjs.com/components/all/"
            min-width="164"
            rel="noopener noreferrer"
            target="_blank"
            variant="text"
          >
            <v-icon
              icon="mdi-view-dashboard"
              size="large"
              start
            />

            Components
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn
            color="primary"
            href="https://vuetifyjs.com/introduction/why-vuetify/#feature-guides"
            min-width="228"
            rel="noopener noreferrer"
            size="x-large"
            target="_blank"
            variant="flat"
          >
            <v-icon
              icon="mdi-speedometer"
              size="large"
              start
            />

            Get Started
          </v-btn>
        </v-col>

        <v-col cols="auto">
          <v-btn
            href="https://community.vuetifyjs.com/"
            min-width="164"
            rel="noopener noreferrer"
            target="_blank"
            variant="text"
          >
            <v-icon
              icon="mdi-account-group"
              size="large"
              start
            />

            Community
          </v-btn>
        </v-col>
      </v-row>
    </v-responsive>
  </v-container>
</template>

<script setup>
//
</script>

```

所以最後可以看到，我們在進入一開始的頁面，其實是一層接一層，中間有透過Router，又有透過import Component的方式，就像堆積木一樣，將各個組件堆叠，最後呈現出一個主頁在各位面前。

雖然看起來很複雜，但是在我們實際開發的過程中，我們用了幾次過後就會很熟悉了，所以各位千萬不要害怕。

本篇終。