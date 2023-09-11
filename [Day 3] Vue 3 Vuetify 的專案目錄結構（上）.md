# [Day 3] Vue 3 / Vuetify 的專案目錄結構（上）

今天是第三天，我想在正式進入“寫程式”這個步驟之前呢，先跟大家介紹當我們新建一個Vue3專案（我們示範是直接用Vuetify）的時候，它一開始的目錄結構的樣子。

爲什麽這件事情如此重要呢？因爲我從大學時期以來到工作上就發現很多人其實他雖然可以寫得出一些程式碼，但是在開發一整個專案的時候呢，會搞不懂哪些東西應該要放在哪裏，或是哪些工具我可以在哪邊找到，某些設定要去哪裏弄等等。到最後就算是開發出了一些簡單的功能，但是後期基本無法去維護，可能有些東西它的設定方式也是有問題的，但是開發人員自己不知道。

# **所以現在我們今天可以來探討一個全新的Vue3的結構目錄是什麽樣子的。**

首先打開一開始製作好的專案：

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542m8dZtxexJa.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542m8dZtxexJa.png)

我們可以看到，左邊的是我們將專案運行起來的畫面，右邊是程式碼的部分。

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542IrTra55SSa.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542IrTra55SSa.png)

我們仔細一個一個看，第一個node_modules我們先放在後面不談，我們可以先來討論一下public目錄是幹什麽用的。

# **（外層）public目錄**

當涉及到Vue.js項目中的public文件夾時，它通常用於存放不需要經過Webpack（或是Vite等等工具，可以看到我們新建的目錄底下最下面那一個vite.config.js，vue3現在都是用Vite來處理，那就是用來做一些打包設定的）構建處理的靜態資源，這些資源將直接覆制到最終的構建目錄（通常是dist）中。這意味著這些資源不會被打包或壓縮，而是以原樣提供給最終用戶。

然而現在有一個問題來了，那就是爲什麽我們現在沒看到dist文件夾？原因很簡單，就是因爲我們還沒“build”這個專案，當你開發完成一個階段，要把這個專案部署到一個伺服器的時候，我們會需要將專案建構起來，所以我們可以直接在termilan上輸入`npm run build`，這樣dist文件夾就會出現在node_modules文件夾的上方。

# **（外層）src目錄**

再來我們展開src文件夾，可以看到這個目錄底下有很多的子目錄：

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542SdpR4fJvoC.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542SdpR4fJvoC.png)

# **（子層）src/assets目錄**

我們通常將src/assets文件夾用於存放應用程序中的靜態資源，這些資源可能包括但不限於：

1. 圖像：可以將應用中使用的圖像文件（如JPEG、PNG、SVG等）放在src/assets/images文件夾中。這些圖像可以在Vue組件中引用，例如用於顯示產品圖片、用戶頭像等。
2. 字體文件：如果需要在應用中使用自定義字體，可以將字體文件（如TTF、OTF、WOFF等）放在src/assets/fonts文件夾中。這樣，可以通過CSS引入並在應用中使用這些字體。
3. 其他靜態文件如video，mp3，json文件……

# **（子層）src/components目錄**

![https://ithelp.ithome.com.tw/upload/images/20230908/201625422dlAAdeMCv.png](https://ithelp.ithome.com.tw/upload/images/20230908/201625422dlAAdeMCv.png)

components目錄通常用於存放自定義的Vue組件（component）。Vue組件是Vue.js應用程序的構建塊，用於封裝和組織應用的不同部分。通過將相關功能和UI組合到單獨的組件中，可以使代碼更具可維護性、可重用性和可測試性。

如果要我用一般人能夠聽得懂的説法，那我擧個例子大家就會很清楚了：

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542SoLNyYLA8Z.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542SoLNyYLA8Z.png)

上面圖片顯示的是一個很常見的網頁的結構，比如説現在我們所在的網站 ( [https://ithelp.ithome.com.tw](https://ithelp.ithome.com.tw/) ) 我們的大部分網頁的排版其實都會是幾種固定的樣式，那我們很常用的一種辦法就是我們希望我們的一個頁面，可以分成很多種模塊，比如説卡片、對話框、表單等等，可以參考Vuetify的元件（或稱組件）：

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542BOF54Law0H.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542BOF54Law0H.png)

( [https://vuetifyjs.com/en/components/all/](https://vuetifyjs.com/en/components/all/) )

當我們將這些很常用到的“視圖”模塊化后，我們在開發其他頁面的時候也可以共用這些模塊（component）

在Vue裏，我們的component會包含他自己的畫面、如果他有一些功能要實現，也會將功能寫在同一個.vue文件

```sql
src/
|-- components/
|   |-- Button.vue
|   |-- Navbar.vue
|   |-- Sidebar.vue
|   |-- ...
```

# **（子層）src/layouts**

![https://ithelp.ithome.com.tw/upload/images/20230908/201625428WR7sw79Hf.png](https://ithelp.ithome.com.tw/upload/images/20230908/201625428WR7sw79Hf.png)

"layout"文件夾通常在前端開發中用於存放佈局的相關代碼，是一種常見的模式，用於管理應用程序的整體布局結構和外觀。

在許多前端框架和library中，包括Vue、React、Angular等，都有類似的布局概念，通常用於以下目的：

1. 頁面結構和布局：布局組件通常包含應用程序的整體頁面結構，例如頭部（header）、導航欄（nav）、側邊欄（aside）、頁腳（footer）等。它們定義了應用程序的整體布局，使得頁面具有一致的外觀。
2. 組織和覆用代碼：通過將佈局組件放置在一個專門的文件夾中，可以更容易地組織和維護應用程序的佈局代碼。這些組件可以在不同頁面之間重覆使用，從而提高了代碼的可維護性和可重用性。
3. 中心化管理布局邏輯：有一些佈局相關的邏輯，例如導航菜單的狀態管理、側邊欄的顯示/隱藏等。這些邏輯可以在布局組件中集中管理，使得應用程序更容易維護。在Vue裏，可以使用佈局組件來定義整個應用程序的外觀，同時在路由中為不同的頁面指定佈局。這讓我們在需要更改頁面的布局時，無需重寫每個頁面的代碼。

# **（子層）src/plugin**

我們打開這個文件夾，可以先看到裏面有三個js檔案：

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542hSyMnmWLeV.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542hSyMnmWLeV.png)

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542u5qHqYO80B.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542u5qHqYO80B.png)

index.js

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542HOaTUbDFxr.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542HOaTUbDFxr.png)

vuetify.js

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542V2ExpR29D6.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542V2ExpR29D6.png)

webfontloader.js

在這個文件夾底下，其實就是用來安裝一些外挂程式的地方，我們看index.js裏面，這一段程式碼：

```php
export function registerPlugins (app) {
  loadFonts()
  app
    .use(vuetify)
    .use(router)
}

```

上面那段程式碼就是説，我今天用一個function（方法）叫做registerPlugins，這個function裏面可以帶進來一個東西，這個東西我們叫做“app”，當我們進入這個程序的時候，我們會將一個Vue實體用一個.use()的方法，將我們要的外挂給這個Vue實體裝上。如一開始他就給我們裝了“vuetify”和“router”。

至於那個webfontloader.js，裏面就是加載了一些谷歌的字體進來（你之後有想要加的字體，也可以上網自己找，用api的方式引進也可以）

# **（子層）src/router**

當你構建一個網站或應用程序時，用戶需要能夠在不同的頁面之間切換，就像在傳統的網站上點擊鏈接一樣。Vue Router是一個Vue.js的官方插件，它讓你能夠在Vue項目中創建這種頁面之間的導航。

router的目錄包含以下內容：

1. 路由配置文件（通常是"index.js"）：這個文件包含了應用程序的不同頁面和URL之間的映射關系。你可以在這裏定義哪個URL對應哪個頁面。例如，"/about" 的URL可能對應於"About"頁面。
2. 路由守衛和導航鉤子（hooks）：有時我們需要在用戶切換頁面時執行一些特殊的任務，如檢查用戶是否登錄或在頁面切換時添加動畫效果。路由守衛和導航鉤子允許我們在這些時刻執行自定義代碼。想象一下自己是不是有過輸入一個網址，但是畫面顯示“404”的畫面，我們可以自己去定義，當用戶去到一個我們不存在的頁面的時候，就會跳轉到某個畫面上。

# **（子層）src/style**

接下來可能有一些學過Vue的小夥伴就會好奇，明明vue官方也推薦用SFC（Single File Component）的方式去寫Vue（像下面這樣）：

```
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="changeMessage">Change Message</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello, Vue!'
    };
  },
  methods: {
    changeMessage() {
      this.message = 'New message!';
    }
  }
};
</script>

<style scoped>
/* 样式仅在当前组件内有效 */h1 {
  color: blue;
}
</style>

```

那爲什麽還需要另外去用一個style文件夾呢？其實這個style文件夾和我們一般的用來存放css的文件夾不是同一個用處。當我們需要寫一些css的時候，我們還是一樣將css寫在同一個Vue文件上，而這個目錄裏面是存放.scss文件，那這個是什麽呢？原來我們現在使用的css有的時候已經無法滿足需求了，所以我們需要一些“加强版css”，這個東西叫做sass：

Sass 是一種 CSS 預處理器，它引入了一些增強 CSS 編寫的功能，如變量、嵌套規則、混合、繼承等。Sass 的主要目的是提高 CSS 代碼的可維護性和可讀性，以及減少代碼冗余。Sass 代碼需要通過編譯器將其轉換為純 CSS，然後才能在網頁上使用。

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542f7FeUGJExH.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542f7FeUGJExH.png)

而我們看到的.scss格式的文件，其實是：

SCSS（Sassy CSS）是Sass（Syntactically Awesome Stylesheets）的一種CSS預處理器。與Sass一樣，SCSS也為CSS提供了一些額外的功能和語法，以增強樣式表的編寫體驗。SCSS與Sass之間的主要區別在於它們的語法風格。

SCSS的語法采用了類似於普通CSS的大括號和分號的寫法，與Sass的縮進風格不同。這使得許多開發者更容易遷移到SCSS，因為它更接近原生CSS。

所以在這個styles文件夾底下，我自己的習慣是只會把它用來複寫Vuetify的一些設定，像是Vuetify的其中一個components `v-button`，默認情況下，會把button裏面的字體全部轉成UpperCase，如果要改成顯示原本的大小寫，就需要到setting.scss上面去覆寫

# **（子層）src/view**

![https://ithelp.ithome.com.tw/upload/images/20230908/201625421QwtE2pZ77.png](https://ithelp.ithome.com.tw/upload/images/20230908/201625421QwtE2pZ77.png)

這個來説相對容易理解，就是我們的顯示的東西，最主要的部分（`<main></main>`），在這邊將各個component組裝起來達到我們要顯示的效果

好的，那我們今天先告一段落，剩下的不在文件夾裏面的資料，如：

![https://ithelp.ithome.com.tw/upload/images/20230908/20162542COffW6dFHZ.png](https://ithelp.ithome.com.tw/upload/images/20230908/20162542COffW6dFHZ.png)

這些和node_modules文件夾，就留到明天再和大家分享！

本篇終。