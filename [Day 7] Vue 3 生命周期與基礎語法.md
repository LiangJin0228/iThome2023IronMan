# [Day 7] Vue 3 生命周期與基礎語法

# **生命周期Lifecycle Hooks**

生命周期是在Vue.js和其他一些前端框架中非常重要的概念之一，它涉及到Vue組件的創建、更新和銷毀過程。當你理解Vue的生命周期時，你可以更好地掌握應用程序的行為，管理數據和執行適當的操作。

---

Vue 3的生命周期包括以下主要階段：

- 創建（Creation）：beforeCreate: 在實例被創建之前被調用，此時實例的選項和數據觀察（data observation）都還未初始化。created: 在實例創建完成之後立即被調用，實例已經完成了數據觀察（data observation），但尚未掛載（mounted）到DOM上。

---

- 掛載（Mounting）：beforeMount: 在實例被掛載到DOM之前被調用，此時實例的模板編譯完成，但尚未將其插入到DOM中。mounted: 在實例被掛載到DOM之後立即被調用，這是你可以訪問DOM元素的地方。

---

- 更新（Updating）：beforeUpdate: 在數據更新之前被調用，此時虛擬DOM處於重新渲染階段，但尚未應用新的數據。updated: 在數據更新並且虛擬DOM重新渲染和差異更新完成之後被調用。

---

- 卸載（Unmounting）：beforeUnmount: 在實例被卸載之前被調用，通常在v-if條件渲染中使用。unmounted: 在實例被完全卸載並且不再存在於DOM中時被調用。

---

- 錯誤處理（Error Handling）：errorCaptured: 用於捕獲子組件中的錯誤，當子組件拋出錯誤時被調用。

---

這些生命周期鉤子（Lifecycle Hooks）允許你在Vue實例的不同階段執行代碼。它們是Vue的重要特性，允許你在需要的時候執行初始化代碼、訂閱數據變化、進行畫面更新等操作。

一樣，看不懂沒關係，我們來個範例爲大家解釋：

假設你正在開發一個簡單的新聞閱讀應用，當新聞列表頁面被訪問時，我們需要從服務器加載新聞數據：

```
<template>
  <div>
    <h1>最新新聞</h1>
    <ul>
      <li v-for="newsItem in newsList" :key="newsItem.id">{{ newsItem.title }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newsList: []
    };
  },
  async beforeMount() {
// 在挂載之前，從服務器加載新聞數據try {
      const response = await fetch('https://api.example.com/news');
      const data = await response.json();
      this.newsList = data;
    } catch (error) {
      console.error('加載新聞數據時出錯：', error);
    }
  }
};
</script>

```

像是這樣，我們就可以在挂載到div上面之前，發送請求截取最新的資料顯示在畫面上，至於爲什麽是beforeMount呢？我們可以假設一下，如果你今天有很多個頁面，每個頁面裏面也有一堆子頁面，你每個頁面都需要從遠端服務器索取資料，可能會導致服務器沒辦法及時處理真正需要被優先顯示出來的資料，所以我們都希望當用戶點擊跳轉到某個頁面時，才加載相關的資料進來，這樣可以優化用戶的體驗。

# **基礎語法**

首先，我們先不討論各位使用的是CDN的方式引入還是直接使用Vite來創建一個Vue程式，這邊爲了教學上的方便，我先暫時使用CDN的方式來進行教學，若是你看到爲什麽我的文件副檔名是.js而不是.vue，不要覺得奇怪。

如果是有一點點基礎的，就知道可以直接創建.vue文件（各位若是之前就跟著我的文章做的話）。

- 步驟 1: 引入 Vue.js
    
    首先，確保你在HTML文件中引入了Vue 3庫：
    
    `<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>`
    
- 步驟 2: 創建 Vue 實例
    
    在你的JavaScript代碼中，創建一個Vue實例，並定義一些資料。這些資料將用於後續的模板渲染。
    

```kotlin
const app = Vue.createApp({
  data() {
    return {
      message: "歡迎使用Vue 3！",
      imageURL: "https://example.com/image.jpg",
      inputValue: "",
      todos: ["學習Vue", "建立應用", "享受Vue"]
    };
  }
});

```

- 步驟 3: 將資料顯示在模板上使用双花括号（{{ }}）將資料顯示在HTML模板中：

```
<div id="app">
  <h1>{{ message }}</h1>
  <img :src="imageURL" alt="圖片" />
  <input v-model="inputValue" placeholder="輸入文字" />
  <ul>
    <li v-for="(todo, index) in todos" :key="index">{{ index + 1 }}. {{ todo }}</li>
  </ul>
</div>

```

這裡，我們使用了插值{{ }}、v-bind 還有它簡寫（: ）和 v-for 指令來顯示資料。

一般顯示在html上面渲染出來的東西，就用插值的方式（兩個大括弧）；

v-bind是當你想用data()裏面的資料作爲html tag的某個參數時，如`<img :src="imageURL" alt="圖片" />"`這樣，就用v-bind，正式寫法是`<img v-bind:src="imageURL" alt="圖片" />`,但是因爲太常用到，我們都習慣簡寫。

再來是for loop，我們可以簡單地用一般的for迴圈的方式如：`v-for="todo in todos`,這裏也很方便的提供了index的功能，如`v-for="(todo, index) in todos`，假設你的`todos`裏面有三個，index就會是[0，1，2]

- 步驟 4: 雙向綁定你可以使用 v-model 指令實現資料的雙向綁定。當用戶輸入內容時，資料將自動更新。

```
<input v-model="inputValue" placeholder="輸入文字" />

```

我們之前在寫form的時候，是不是都會給每個input一個name，當你按下submit的時候將資料post到後端，由後端接受name對應的value，但是現在Vue使用v-bind方法，你只需在script裏面定義data()的部分，將變數名稱塞到v-bind將這個input綁定到該變數，用戶輸入的東西，你都可以即時得到。

- 步驟 5: 條件渲染你可以使用 v-if 指令來根據條件渲染內容。例如，當陣列為空時顯示一條消息：

```xml
<p v-if="todos.length === 0">沒有待辦事項。</p>

```

- 步驟 6: 使用 v-showv-show 指令與 v-if 類似，但有一個重要的區別。v-show 始終將元素渲染到DOM中，但可以通過CSS的 display 屬性來控制其可見性。

```xml
<p v-show="todos.length === 0">沒有待辦事項。</p>

```

`v-show 始終將元素渲染到DOM中`這句話是説，我今天如果if判斷為false，我就不會渲染到DOM，但是show方法是會渲染到DOM但是不會顯示，你可以透過css爲他加一個過渡動畫。

總之，這個簡單的Vue 3教學向你展示了如何將資料顯示在模板上，設置HTML標籤的屬性，(input)實現雙向綁定，使用for迴圈渲染陣列，以及如何使用 if 和 show 來進行條件渲染。

請記得，在使用這些指令的是要要加'v-'（eg. v-if， v-for）。

明天再教各位如何處理事件(event)

本篇終。