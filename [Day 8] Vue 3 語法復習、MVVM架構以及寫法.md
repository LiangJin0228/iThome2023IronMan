# [Day 8] Vue 3 語法復習、MVVM架構以及寫法

# **復習**

首先，在開始學新東西前，讓我們先來復習之前學過的内容。

可以參考我的Github repository（雖然現在只寫到lesson2，那是之後準備拍教學影片用的）（ [https://github.com/LiangJin0228/learnVueWithLiangJin](https://github.com/LiangJin0228/learnVueWithLiangJin) ），搭配這系列的文章來學習：

lesson1.js:

```objectivec
// const app = Vue.createApp()// 這個方法表示創建一個Vue應用,這個應用叫做appconst app = Vue.createApp({
// 在這邊定義資料,在前端可以直接使用key將value提取出來// 但是要記得data裏面的資料不能直接當成html tag的屬性// 如 <div class="{{ occupation }}"></div> 這樣是不被允許的// 正確的方式應該是使用v-bind指令
    data() {
        return {
            name : "LiangJin",
            occupation : "full-stack developer",
            age : 23,
// 用這個link當作示範使用v-bind指令
            link : "https://www.google.com",
// 用這個todo來示範如何使用
            todos : ["洗澡","吃飯","睡覺"],
// 用這個objectV_for來示範如何用for loop來遍歷數組物件// 也順便展示在data裏面的資料不一定是string,也可以是int, boolean...
            objectV_for :[
                {
                    id : 1,
                    name : "小可愛",
                    class : "高一理科",
                    bool : true
                },
                {
                    id : 2,
                    name : "冷漠哥",
                    class : "高三文商科",
                    bool : false
                }
            ],
// 用這個books來示範v-if, v-else-if, v-else 搭配v-for來使用
            books : [
                {
                    title : "我的第一本書",
                },
                {
                    title : "English Book",
                },
                {
                    title : "Essentials of the Java Programming Language, Part 1",
                }
            ]
        }
    }
});
// 下面這一行代表的是將最上面建的const app = Vue.createApp();這個東西和html上的某個被取名id="app"的元素挂載(連接)在一起
app.mount("#app");

```

lesson1.html:

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link rel="stylesheet" href="css/lesson 1.css" />
        <script src="lib/vue3.js"></script>
        <title>Vue Practice</title>
    </head>
    <body>
        <div id="app">
            <h1>
<!-- 通過大括號,直接使用vue裏面的data() --><!-- 在index.js裏面,data會返回一個數組,裏面的内容是key:value --><!-- 我們只要簡單將key放在大括號裏面,就會顯示出value -->
                Hi, my name is {{ name }}, I'm a {{ occupation }} and my age is
                {{ age }} years old.
            </h1>
            <br />
<!-- 下面這個是正式寫法，但是因爲v-bind太常使用，因此我們可以像下面第二行只放一個":" --><!-- 推薦是使用簡寫形式 --><!-- <a v-bind:href="link">Google首頁</a> --><a :href="link">Google首頁</a>
            <br />
            <ul>
<!-- 這樣是一個標准的無序列表 --><!-- <li v-for="todo in todos">{{ todo }}</li> --><!-- 這樣就會在它的前面加上index --><li v-for="(todo,index) in todos">{{ index+1 }}.{{ todo }}</li>
            </ul>
            <br />
            <ul>
<!-- 下面示範怎麽用v-for loop來遍歷數組對象 --><!-- object是在這個loop裏面定義的, in後面的objectV_for對應的是data裏面的key --><!-- 下面用object.XXX的方式使用它的屬性 --><li v-for="object in objectV_for">
<!-- 注意這個checkbox,他的屬性checked使用的是v-bind指令 --><input type="checkbox" :checked="object.bool" />{{ object.id
                    }}   {{ object.name }}   {{ object.class }}
                </li>
            </ul>
            <br />
<!-- 接下來要展現的是 v-if / v-else-if / v-else功能,就和一般程式語言或JS一樣的寫法 --><!-- 返回True:{true, 1, 非空字符串} --><!-- 返回False:{false, 0, undefined, null, ""} --><!-- 要注意的是,v-if, v-else-if, v-else必須是緊挨著才能奏效 --><p v-if="books.length === 0">一本書都沒有……</p>
            <h1 v-else-if="books.length === 1">{{ books[0].title }}</h1>
            <ul v-else>
                <li v-for="book in books">{{ book.title }}</li>
            </ul>
            <br />
<!-- 與v-if 類似的還有v-show 指令 --><!-- 如果v-if 的條件不滿足，那麽就不會顯示/執行後面的内容 --><!-- 但是使用v-show 的話,即使條件不滿足，也會將HTML標簽渲染出來，只是會加上css的display:none 屬性 --><!-- 那爲什麽需要有v-if 了還要有這個判斷指令呢？ --><!-- 因爲有些時候我們需要做一些css的過渡動畫，比如説讓這個div慢慢淡入出現，就需要將這個HTML標簽事先渲染出來，才能夠有過渡的動畫效果 --><!-- 還有要注意的是,v-show 不能搭配v-else做使用,就只有一次判斷而已,所以要有多個判斷就用多個v-show --><p v-show="books.length === 0">一本書都沒有……</p>
            <ul v-show>
                <li v-for="book in books">{{ book.title }}</li>
            </ul>
        </div>
        <script src="lib/lesson 1.js"></script>
    </body>
</html>

```

lesson2.js:

```kotlin
const app = Vue.createApp({
    data() {
        return {
            click : false,
            countDown : 5,
            timer : null
        };
    },
    computed: {
// 計算屬性，能夠使用計算的方式動態回傳一個值或數組
        isClicked() {
            return this.click ? "隱藏 " + this.countDown : "展開";
        }
    },
    methods: {
        clickEvent() {
            this.click = !this.click;
        }
    },
    watch: {
// watch方法是監視你的data裏面的值，所以你的data裏面的key叫什麽名字，watch就必須一模一樣// 下面這行的意思就是,目前我們的click預設是false，但是當我們按下按鈕，它會變成true// 所以newValue就是true, oldValue就是false
        click(newValue, oldValue) {
            if (newValue) {
                this.countDown = 5;
                if (this.timer) {
                    clearInterval(this.timer);
                    this.timer = null;
                }
                this.timer = setInterval(() => {
                    this.countDown -= 1;
                    if (this.countDown === 0) {
                        this.click = false;
                        clearInterval(this.timer);
                        this.timer = null;
                    }
                }, 1000);
            }
        }

// 另外，當我們在watch一個data物件的時候，如果我們觀察的是整個物件的變化，就可以用上面的寫法// 但是，如果我們要監聽的是一個物件裏面的任何一個value的變化，那我們就要加上`deep: true`屬性// data() {//     return {//         userData: {//             name: 'John',//             age: 30,//         },//     };// }// 像這樣如果我們有修改userData裏面的屬性的話，我們就要使用另外一種寫法：// watch: {//     userData: {//         handler(newVal, oldVal) {//         console.log('userData changed');//         },//         deep: true,//     },// },// watch下面一樣放data()裏面的名稱，然後裏面塞入一個handler方法，并且在handler方法外面、userData裏面加入deep: true 屬性
    }
});
app.mount('#app');

```

lesson2.html:

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link rel="stylesheet" href="css/lesson 2.css" />
        <script src="lib/vue3.js"></script>
        <title>Vue Practice</title>
    </head>
    <body>
        <div id="app">
            <div>
<!-- 從下面可以看出,我將click事件放到一個h1標簽上 --><!-- 所以并不是只有button, input之類的可以監聽事件 --><!-- @click事件中，那一行的意思是: 當你按click之後,本來的click就是false(在Vue的data()裏面)右邊的"!click"會在每次點擊的時候將click的值反轉 --><!-- div的部分則依據click的值來選擇是否顯示 --><div v-show="click">你剛剛點了下面的h1</div>
<!-- 這邊使用的是運算符，而下方第二行用的是計算屬性(computed()方法) --><!-- <h1 @click="click = !click">{{ click ? "隱藏" : "展開" }}</h1> --><h1 @click="click = !click">{{ isClicked }}</h1>
                <br />
<!-- 我們可以使用另外一種方法，那就是將v-on 事件的處理包裝成一個methods --><div>
                    <div v-show="click">這個是使用method事件的div</div>
                    <button @click="clickEvent">{{ isClicked }}</button>
                </div>
            </div>
<!-- 重點整理 --><!-- 計算屬性:緩存結果 如果多次使用,data屬性沒有變化,會直接從緩存拿出來用,性能方面好一些(換句話説，只有在data()裏面的值變化時才計算) --><!-- 方法:不會緩存結果  每一次執行都會重新計算,并且計算屬性只有newValue和oldValue,但是方法可以傳遞參數 --><!-- 可以在computed 裏面使用this.methodName()來使用定義好的方法 --><!-- 計算屬性因該是被當作data一樣來做使用 --><!-- 方法通常作爲事件監聽, 和一些很常用到的邏輯將它們抽離出來,當作一般的js函數使用 --></div>
        <script src="lib/lesson 2.js"></script>
    </body>
</html>

```

# **MVVM(Model - View - ViewModel)**

可能這個詞對新手來説比較陌生，可能聽過，但是沒有實際感受過，我們今天就和大家來講解一下為什麽說Vue是基於MVVM的原因：

1. 分離視圖和資料： MVVM模式的核心思想是將應用程序的用戶界面（View）和應用程序的資料（Model）分離開來，Vue也采用了這一思想。Vue的組件充當了ViewModel的角色，它們負責管理視圖和資料之間的交互，使開發人員可以專注於UI和數據的不同方面。
2. 雙向數據綁定： 在MVVM中，資料綁定是一個關鍵概念。Vue提供了雙向資料綁定，這意味著當資料模型發生變化時，視圖會自動更新，反之亦然。這種自動資料綁定使得開發人員不需要手動更新DOM，從而提高了開發效率。
3. 模板系統： MVVM通常包括一個模板（template）系統，用於定義用戶界面的結構和布局。Vue使用類似HTML的模板語法，開發人員可以在模板中聲明視圖，然後Vue會根據數據模型自動更新視圖，這與MVVM的思想一致。
4. 響應式： 在MVVM模式中，資料模型應該是響應式的，即當數據變化時，應該通知視圖更新。Vue的響應式系統確保了這一點，開發人員可以輕松地創建響應式資料模型，而不必擔心手動管理資料更新。總之，雖然Vue不是MVVM模式的嚴格實現，但它在架構和設計理念上受到了MVVM的啟發，因此通常被稱為MVVM框架。這有助於開發人員更容易理解和使用Vue，並能夠充分利用其提供的資料綁定和響應式特性來構建現代的交互式Web應用程序。

同樣，我們用人話再説一遍（總結：`<script>`部分就當作是model，`<template>和<style>`就當作是view的部分）：

### **Model:**

你可以想象成是`<script>`裏面的`data()`、`computed`、`method`和`watch`的部分，以及生命周期。你們現在可能會很疑惑，爲什麽除了data()看起來比較像是model的東西，其他什麽`computed`、`method`和`watch`看起來就像是MVC架構裏面的Controller（控制器，處理業務邏輯的部分，如果不知道也沒關係），怎麽也會把它歸類爲model呢？原來啊，我們把‘你怎麽取得資料’這件事也當成了model層來看待，我們可能會寫一些method（或稱function，函數）來取得我們要的資料，或是透過computed來計算（或篩選）我們要的資料，或是watch（監聽）一些事件的發生來改變資料，或是我們希望在頁面挂載上去（mounted，生命周期的其中一個）的時候加載一些信息，所以這些都是我們可以取得或改變資料的一種手段，在Vue裏面我們都把它們當作是model層面。

### **View:**

把它想象成是`<template>`和`<style>`的部分

### **View-Model：**

其實這層就是Vue這個框架本身

---

我們先來看一張圖片：

![https://ithelp.ithome.com.tw/upload/images/20230913/20162542hWBHXqhhu9.png](https://ithelp.ithome.com.tw/upload/images/20230913/20162542hWBHXqhhu9.png)

透過這張圖我們可以看到，view透過和view-model進行`雙向綁定`，然後view-model和model層再進行溝通。

我知道這句話可能對新手來説非常的抽象，難以理解，我們現在用例子説一遍：

想象一下，如果我現在要開發一個班級管理的系統，一個老師可以管理自己的班級的學生，查看用他們的基本資料、出缺勤記錄、功課繳交記錄，是否交了班費等……那這些資料一般來説都會存在資料庫對吧。

我今天打開這個網站，登入進這個系統之後，我需要能夠在dashboard看到我的班級的狀態（如，所有學生的資料），這時候，我是不是就要從資料庫去將當前這個用戶（老師）所管理的班級的所有學生的資料抓出來，這就是model層在做的事。

`model 層： 將需要的資料抓出來`

然後，我在抓了這個資料后（你想象一個excel表格，上面有id，學號，姓名，性別……每一個學生都是一筆資料），我需要將這些東西顯示在畫面上，所以我透過Mustache語法（兩個大括號'{{ }}'）將資料顯示在view層上。

`view 層： 將model的資料顯示出來`

這個時候你就會發現，我的`view-model`層去哪裏了？

其實這個view-model層就是Vue框架本身。我們如果不是用Vue的情況下，我的data和html上的顯示是無法雙向綁定的，比如説，我今天從資料庫抓了需要的資料存在model裏面，然後我想要將資料顯示在畫面上，我需要在JavaScript裏面寫：

---

（步驟）

1. 抓取div id=XXX的Dom元素（document.getElementByID()類似這樣的方法）（簡稱div original）
2. 新增一個新的div （簡稱div a）
3. 將我的model塞進去這個div a
4. 將div a塞進div original

---

想象一下，這還只是一個非常簡單的例子，我就有至少四個步驟，如果我的資料每一個欄位都要去反復操作這些東西，你的代碼會變得又長又臭，且開發很沒用效率。

但是Vue幫我們解決這個問題，我們不用去寫這些將“資料”顯示到“畫面”的邏輯，我們只要專注在取得“資料”，和美化“界面”這兩件事就好，Vue會幫我們把資料實時更新到畫面上；反過來説，我在一個form表單的欄位填入的資料（如姓名、email），Vue也會即時的將資料更新到model上，我不用去寫一堆.getValue()的方法，所以這個在Vue的官方文檔上面把它叫做`關注點分離` `關注點分離` `關注點分離`!(就是我只需要專注在model和view上)

如果你回去看我昨天的文章（ [https://ithelp.ithome.com.tw/articles/10317101](https://ithelp.ithome.com.tw/articles/10317101) ），你就會發現，昨天説的`基礎語法`的部分全部都是屬於View的。所以從今天開始你就知道整個Vue的運作原理，相信之後在學習Vue的時候會更輕鬆。

先預告一下，明天真的就會開始説明`data`,`computed`,`method`,`watch`,`lifecycle hooks`等（上一篇文章説的處理事件(event)也是算在這個部分）。

後天開始會教大家怎麽帶參數(props,$emit)等，vue本身的部分就差不多了。

接下來就是套件如vue-router、Vuetify的部分，至於狀態管理（pinia / vuex）則會看情況要不要教。

後面我們會花很多時間在熟悉Vuetify這個UI框架上。

然後大概在第20篇左右的時候會教大家寫一個個人網站（至少會使用到vue-router和Vuetify），以及如何將他們部署到（ [https://pages.github.com/](https://pages.github.com/) ）上(免費！)。

再來就是怎麽去結合後端laravel展開一個小小的專案（教學）。

今天就到這裏吧。

本篇終。