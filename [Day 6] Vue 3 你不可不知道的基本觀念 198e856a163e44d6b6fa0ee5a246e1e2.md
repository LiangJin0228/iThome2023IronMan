# [Day 6] Vue 3 你不可不知道的基本觀念

説個題外話，今天早上才發現，day4和day5標題不小心寫成一樣了……不過沒關係，我們知道内容不一樣就好……

好了，扯回我們的主題。

# **1.SFC? Single-File Components?**

我們先來瞭解一個.vue文件裏面會有什麽東西：

一個比較推薦的寫法是SFC的寫法，全稱的話叫做Single-File Components。這句話是什麽意思呢？也就是我們在寫前端的時候，我們通常不是用框架的前提下，我們會將html，css，JavaScript拆開分城單獨的文件。

比如説，我今天寫一個部落格網站，我可能會有首頁（index）用來顯示熱門話題；探索（explore）頁面來探索各個看板（像是ptt、dcard那樣有分成多個看板），然後可能還有管理自己發佈的文章（manage blog）等頁面，想象一下，我們在用全原生的html,css,js來寫的話，是不是index會分成index.html,index.css,index.js三個文件，然後explore的頁面也是自己的explore.html,explore.css,explore.js三個文件，其他頁面也會有自己的文件……

但是到了Vue不一樣，Vue不是這樣分的。Vue的推薦寫法是單頁式Single-File Components，這個的意思就是，我們盡可能地將一個頁面做區塊上的拆分，將他們的html，css和js寫在同一個.vue文件上，以下是一個範例：

想象一下你有一個首頁的結構樹長這樣：

```
src/
  ├── main.js
  ├── App.vue
  ├── components/
  |     ├── Header.vue
  |     ├── MainContent.vue
  |     └── Footer.vue
  └── assets/
        ├── logo.png

```

你的Header.vue長這樣

```xml
<template>
  <header>
    <img src="@/assets/logo.png" alt="Logo" />
    <nav>
      <ul>
        <li><a href="#">首頁</a></li>
        <li><a href="#">關於我們</a></li>
        <li><a href="#">聯絡我們</a></li>
      </ul>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'Header',
};
</script>

<style scoped>
/* 樣式可根據需要自定義 */header {
  background-color: #333;
  color: #fff;
  padding: 10px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

nav ul {
  list-style: none;
  padding: 0;
  display: flex;
}

nav ul li {
  margin-right: 20px;
}

nav a {
  color: #fff;
  text-decoration: none;
}
</style>

```

你的MainContent.vue長這樣:

```xml
<template>
  <main>
    <section>
      <h1>歡迎來到我們的官網</h1>
      <p>這是一個簡單的示例官網首頁。</p>
    </section>
  </main>
</template>

<script>
export default {
  name: 'MainContent',
};
</script>

<style scoped>
/* 樣式可根據需要自定義 */main {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  font-size: 24px;
}

p {
  font-size: 16px;
}
</style>

```

你的Footer.vue長這樣：

```xml
<template>
  <footer>
    <p>© 2023 我們的官網</p>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
};
</script>

<style scoped>
/* 樣式可根據需要自定義 */footer {
  background-color: #333;
  color: #fff;
  text-align: center;
  padding: 10px 0;
}

p {
  font-size: 14px;
}
</style>

```

現在，你只需要把這三個組件組裝起來，就是一個完整的頁面：index.vue

```xml
<template>
  <div>
    <Header />
    <MainContent />
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue';
import MainContent from './components/MainContent.vue';
import Footer from './components/Footer.vue';

export default {
  components: {
    Header,
    MainContent,
    Footer,
  },
};
</script>

```

這樣做有什麽好處呢？相比於單個單個頁面去寫，這樣的方式更靈活，更好維護，而且最重要的是，我們可以共用會一直用到的組件。這時候問題又來了，我就算不用vue3框架，我用jQuery或是一些library也可以做到啊！

這你們就不懂了，我爲你們總結一下前端框架的幾個好處：

- 組件化開發： Vue 3鼓勵使用組件化的方式來構建應用程序。這意味著可以將UI劃分為獨立的、可重用的組件，使代碼更具組織性和可維護性。這比在jQuery中直接操作DOM更具結構性和可讀性。
- 更好的狀態管理： Vue 3提供了Vuex（現在pinia也逐漸擡頭了），一個專門用於狀態管理的庫。它使得管理應用程序的狀態更容易，尤其是對於大型或複雜的應用程序來說。這在jQuery中通常需要手動維護狀態，而且容易導致混亂。
- 更好的性能： Vue 3引入了許多性能優化，例如虛擬DOM（Virtual DOM）和更快的渲染速度。這意味著當數據變化時，Vue 3將只重新渲染必要的部分，而不是整個頁面。這對於提高應用程序的性能和反應速度很有幫助。
- 生態系統和工具： Vue 3擁有龐大的生態系統，包括許多插件和工具，如Vue Router用於路由管理，Vue CLI用於項目構建，以及第三方庫和組件庫。這些工具和插件可以加速開發過程。
- 社區支持： Vue有一個活躍的社區，定期更新和維護，並提供廣泛的文檔和學習資源。這使得學習和解決問題變得更加容易。

另外，將一個頁面拆成多個組件，也可以避免開發到最後，可能各個js檔案或css檔案裏面設置的東西、或是class起衝突，而且一旦東西變多變複雜起來，要管理和出錯是真的很可怕的一件事。

總之，Vue 3以其組件化開發、狀態管理、性能優化、豐富的生態系統和社區支持等方面提供了許多優勢，使得它成為現代前端開發的一個有力工具。儘管可以使用jQuery等工具進行前端開發，但使用Vue 3可以更高效、更容易地構建現代化、可維護的應用程序。

# **2.Option API? Composition API**

在寫Vue的時候，應該很多人蠻疑惑的是什麽是option什么是composition。

我們在寫vue3的時候其實有兩種風格（注意，是風格，不是說你一定要寫哪一種寫法），當然，官方會比較推薦寫的是Composition API。

無論是Option API還是Composition API，它們都是將一個.vue文件寫成三個區塊，最上面寫script，中間寫template（html），最下面寫css（可能會有一些人的寫法是先寫template，再寫script，這個看你自己喜歡）：

### **Option API（選項式API）：**

特點：Option API 是 Vue 2 中主要的編程方式，它將所有的組件選項（如 data、methods、computed、watch 等）都定義在一個對象中。

優點：Option API 簡單易懂，適合小型項目或初學者。

示例：以下是一個使用 Option API 的簡單示範：

```
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="changeMessage">更改消息</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "這是一條消息"
    };
  },
  methods: {
    changeMessage() {
      this.message = "消息已更改";
    }
  }
};
</script>

```

可以看到它在script裏面，分成了好幾個區塊（選項，option），也就是在script的部分裏面分成了data(),method()，除此之外，還有其他選項（option）如watch（類似js裏面的什麽什麽listener，只不過監聽對象是data），computed（也是監聽data）等選項，所以你有用到的時候，你就將這些選項寫出來，所以才叫option api。

我們再開composition api怎麽寫：

特點：Composition API 是 Vue 3 中引入的一種新編程方式，它允許你根據功能將代碼組織成多個獨立的函數。這樣可以更好地重用代碼和組織邏輯。

優點：Composition API 更靈活，使代碼更易於維護和測試，特別適用於大型項目或需要複雜邏輯的場景。

示例：以下是一個使用 Composition API 的簡單示範：

```
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="changeMessage">更改消息</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const message = ref("這是一條消息");
    const changeMessage = () => {
      message.value = "消息已更改";
    };

    return {
      message,
      changeMessage
    };
  }
};
</script>

```

Composition API就好像我們在其他程式如java，c++，php等class的寫法，我不用將變數宣告在data區域，也不用將function宣告在method區域，所以我們可以將做同一件事情的變數和方法放在一起。

總結，Option API 和 Composition API 都是 Vue 中的兩種不同方式來構建組件。Option API 更傳統且容易理解，而 Composition API 則更強大和靈活，特別適合大型項目。初學者可以先從 Option API 開始，然後再漸漸了解 Composition API，根據項目的需要來選擇使用哪一種。

# **3.響應式？框架**

Vue 3 是一個響應式（Reactive）的JavaScript框架，這意味著它能夠自動追蹤數據的變化並將這些變化反映到你的應用界面中。以下是為什麼 Vue 3 是響應式的解釋：

1. **簡單性**：Vue 3 的響應式系統使數據綁定和更新界面變得簡單明瞭。你只需聲明你的數據，然後Vue會自動處理界面的更新。這簡化了代碼的編寫，減少了手動DOM操作的需求。
2. **組件封裝**：Vue的組件將數據和界面結合在一起，這意味著每個組件都可以具有自己的狀態（數據）和視圖（模板）。響應式系統使得在組件中管理狀態變得容易，並確保了數據和界面的同步。
3. **性能優化**：Vue 3 使用了一個稱為“Proxy”的特性，它允許Vue更有效地追蹤數據的變化，從而提高了性能。Vue還引入了一個“Fragments”概念，可以更高效地渲染多個組件。這些優化確保了響應式數據更新的效率。
4. **彈性和可擴展性**：Vue 3 的響應式系統是高度可配置的，允許你自定義數據的變化追蹤和處理邏輯。這使得它非常靈活，能夠應對各種不同的應用場景。

總之，Vue 3的響應式是其設計的核心特點之一，它使得數據和界面之間的關係更加直觀和易於管理，同時也提供了性能優化和彈性配置的好處。這使得Vue 3成為一個流行的前端框架，適用於各種Web應用程序的開發。

是不是聽的半懂半不懂，沒關係，各位今天肯花時間來研究，就表示你們在進步了。説個題外話，我是覺得說身爲一個工程師（或准工程師，或你想要成爲工程師），是一定要去面對這些説的很標准的文件的（你各位可以去看看官網的文檔），不只是vue，可以去看其他的如laravel，java等的document，很多時候沒辦法理解沒關係，看多幾遍，再配合上網找資料，嘗試慢慢消化它。

拉回我們的話題，這個響應式框架的意思就是，我們的在`<script>定`義出來的變數，我們將它顯示在html上面的時候，如果我們將它的值改變，它會馬上反映到html上，而我們不用去操作dom，如：

```
<template>
  <div>
    <p>數字：{{ number }}</p>
    <button @click="increment">增加數字</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
// 使用 ref 創建一個響應式變量const number = ref(0);

// 定義一個函數來改變數據const increment = () => {
      number.value++;
    };

// 返回響應式變數和函數return {
      number,
      increment
    };
  }
};
</script>

```

我們定義了一個function叫做increment，當button每按一次，數字就會+1，但是我們改變的僅僅是定義的'number'這個變數而已，如果是原生JS的話：

```xml
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>原生 JavaScript 響應式示例</title>
</head>
<body>
  <div>
    <p>數字：<span id="numberDisplay">0</span></p>
    <button id="incrementButton">增加數字</button>
  </div>

  <script>
// 獲取 DOM 元素const numberDisplay = document.getElementById('numberDisplay');
    const incrementButton = document.getElementById('incrementButton');

// 定義數據let number = 0;

// 更新數字顯示const updateNumberDisplay = () => {
      numberDisplay.textContent = number;
    };

// 增加數字的函數const increment = () => {
      number++;
//每次增加，都要去更改numberDisplay.textContent
      updateNumberDisplay();
    };

// 添加點擊事件監聽器
    incrementButton.addEventListener('click', increment);

// 初始化數字顯示
    updateNumberDisplay();
  </script>
</body>
</html>

```

看到了吧，我們在Vue上面只要用兩個大括號'{{ XXX }}'就可以即時顯示我們的變數(option api裏面的data()部分)，而不用去取得id、修改該dom元素内的值……

# **4.Single Page Application(SPA)是什麽？**

簡單來説，就是不刷新頁面，但是可以讓更新的内容顯示在網頁上的一個模式。

我們上面不是有説到我們的Vue是響應式的，可以即時更新我們的變數？

那你想一下我們每次需要刷新頁面的時候都是在做什麽事情？比如説我在首頁點了一篇文章要閲讀，這時候畫面應該顯示那篇文章的標題、内容、作者、留言……，我們只要取得這些資料，將它顯示在當前頁面就好了。

這時候聰明的小夥伴可能就像得到了，我們去跟後端要這些資料，後端傳回json(放在前端就是proxy物件)，當我們接收到這些東西的時候，我們就只需要將他更新到我們早已定義好的html模板(template)，而我們取得資料的方式，也不必透過html form的get方法或post方法（因爲這樣會刷新頁面），而是使用非同步的方法如promise，fetch api，ajax， axios之類的……去跟後端要資料。

可以想象：像Facebook或Twitter這樣的社交媒體網站通常是SPA的典型例子。用戶可以無縫地滾動新聞頁面，點擊鏈接查看詳細信息，而不需要頁面的完整刷新。

明天再教大家關於Vue應用的生命周期，以及if-else，for的寫法。

本篇終。