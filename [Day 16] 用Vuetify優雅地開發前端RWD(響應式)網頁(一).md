# [Day 16] 用Vuetify優雅地開發前端RWD(響應式)網頁(一)

30天已經過去了一半，我們終於！終於！終於！要進入到Vuetify的世界了，不管你是前端工程師，或是UI/UX設計師，如果你的專案有使用到Vue框架的話，多多少少應該都會聽過Vuetify這個框架。Vuetify 是一個流行的 Vue.js UI 框架，它提供了許多預製的 UI 元件和樣式，可幫助你輕鬆建立現代、美觀的 Web 應用程序。Vuetify 由一些擁有豐富而強大的特性的，預定義組件組成。這些特性包括例如動態主題、全局默認值、應用框架等等構成。它以提供給開發者豐富的工具，給用戶良好而豐富的使用體驗為最終目標。

話不多説，我們開始使用Vuetify，如果還沒安裝的話，可以去參考我的第二篇文章[3種方法快速建立 Vue/Vuetify 專案，手把手帶你進入Vue的世界](https://ithelp.ithome.com.tw/articles/10315719),當你創建好一個Vuetify專案后，我們再來進入實作。

首先我們可以參考一下我之前幾篇在說關於Vue-router的文章，我們會將首頁的主要内容放在一個`./src/views/Home.vue`的組件裏面，我們先來決定我們前端的一些樣式：

以我的個人網站爲例子：

![https://ithelp.ithome.com.tw/upload/images/20230921/20162542p9UQAmMcVG.png](https://ithelp.ithome.com.tw/upload/images/20230921/20162542p9UQAmMcVG.png)

我們先不要關心上面那個NavBar，我們先注意我們的主要區域，有發現我的背景放了一張圖片，它沾滿了整個主要的空間。

# **v-parallax**

這個東西是使用了v-parallax這個組件完成的，它和一般的img不一樣，他能做到一種在滑動的時候，背景圖片會有延遲的感覺，就是建立 3D 效果，使影像看起來比視窗滾動得慢。

我們給這個parallax一個src（背景圖片）：

`<v-parallax src="../assets/home-background-banner.jpg" class="align-center fill-height">`

可以發現，我們還在它的後面給了它一些class，這些class其實都是Vuetify裏面幫我們定義好了的，我們只要給他標識上去就可以做到我們平時在寫Style的效果，像是`align-center`其實就是`align:center`，`fill-height`就會使你的高度height=100%。

再來我們希望在這個parallax裏面放一些内容，但是我們去Vuetify官網一看，這麽多Component到底要怎麽選？就讓我娓娓道來。如果是我們平時沒有使用框架的話，我可能就會直接給它一個div，但是我想説我們都用框架了，就盡量使用框架幫我們做好的東西，不然就會失去使用框架的意義。

# **v-app**

我們希望我們的網站是RWD的，因此我們可能會需要一些佈局的容器來讓我們的畫面自動的去RWD，在Vuetify的世界裏面，我們需要在最外層的Component（就是最外層的Vue文件）用一個`<v-app></v-app>`標簽包起來，就像是：

```xml
<template>
    <v-app>
      <v-card
        class="mx-auto"
        width="400"
        prepend-icon="mdi-home"
      >
        <template v-slot:title>
          This is a title
        </template>

        <v-card-text>
          This is content
        </v-card-text>
      </v-card>
    </v-app>
</template>

```

![https://ithelp.ithome.com.tw/upload/images/20230921/20162542Cui4D713c6.png](https://ithelp.ithome.com.tw/upload/images/20230921/20162542Cui4D713c6.png)

這件事情很重要，它是必須的，并且v-app只能在根組件使用、只能有一個，也就是説你可能會把一個頁面拆成很多個組件，但是你們import到最後的root才需要放，其他的組件不用。

# **v-layout**

再來，佈局方面除了最外層要放`<v-app></v-app>`，再來還有`<v-layout></v-layout>`，v-layout 用於建立響應式佈局。它提供了一個靈活的網格系統，以結構化的方式排列和對齊內容。但是我必須說，v-layout這個東西有可能是過時的，因爲我們能在Vuetify2的文檔找到它的很多的使用方式，但是在Vuetify3的文檔裏卻沒有對它進行過多的説明，所以我覺得這個東西可以放，但是不放的話我也可以用`v-row`和`v-col`來搭配使用，我先給大家看用和不用`v-layout`的效果：

使用v-layout:

![https://ithelp.ithome.com.tw/upload/images/20230921/20162542seGzvIvlfB.png](https://ithelp.ithome.com.tw/upload/images/20230921/20162542seGzvIvlfB.png)

不使用v-layout:

![https://ithelp.ithome.com.tw/upload/images/20230921/20162542cR2svrL1xD.png](https://ithelp.ithome.com.tw/upload/images/20230921/20162542cR2svrL1xD.png)

看到他最上方的那個application-bar（藍條部分）的變化了嗎？

但是我還是得說，要使用`v-layout`或是`v-row`和`v-col`搭配使用，完全是看個人習慣和團隊習慣，在我看來都是十分靈活且可以快速建立視圖的組件。

# **v-container**

在説明`v-row`和`v-col`之前，我先來説説一個神器——`<v-container>`，我們一般來説在寫原生的前端的時候會使用到很多的`div`，但自從接觸了Vuetify，我都使用它的`v-container`來當作`div`使用，因爲它能夠自動幫我們做RWD，也就是我們只要把東西放在`v-container`裏面，不管什麽組件他都可以自動適配不同尺寸的熒幕。

v-container 提供了居中和水平填充內容的能力。您也可以使用fluid將容器完全擴展到所有視窗和設備尺寸。

比如説，我可能會使用：`<v-container style="max-width: 1280px;">`，來讓它能夠在v-container内部自動RWD，而不希望它在非常巨大的屏幕上，也被拉伸到非常大。如果是其他的組件，其實大部分都自帶max-width的props，但是不知道爲什麽我覺得v-container也非常需要但是他們卻沒有做，可能是希望我們使用`v-row`和`v-col`。

# **v-row和v-col**

這兩個組件應該從字面意思就看得懂，一個是row，一個是column。必須注意的是，v-col必須是在v-row的裏面。

v-row 是 v-col 的包裝組件。它利用 flex 屬性來控制其內列的佈局和流程。它使用 24px 的gutter 。這可以透過dense來減少，或透過no-gutters完全去除。

我們可以先來看官網的其中一個範例：

```xml
<template>
  <v-container class="bg-surface-variant">
    <v-row justify="start">
      <v-col
        v-for="k in 2"
        :key="k"
        cols="4"
      >
        <v-sheet class="pa-2 ma-2">
          .justify-start
        </v-sheet>
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-col
        v-for="k in 2"
        :key="k"
        cols="4"
      >
        <v-sheet class="pa-2 ma-2">
          .justify-center
        </v-sheet>
      </v-col>
    </v-row>

    <v-row justify="end">
      <v-col
        v-for="k in 2"
        :key="k"
        cols="4"
      >
        <v-sheet class="pa-2 ma-2">
          .justify-end
        </v-sheet>
      </v-col>
    </v-row>

    <v-row justify="space-around">
      <v-col
        v-for="k in 2"
        :key="k"
        cols="4"
      >
        <v-sheet class="pa-2 ma-2">
          .justify-space-around
        </v-sheet>
      </v-col>
    </v-row>

    <v-row justify="space-between">
      <v-col
        v-for="k in 2"
        :key="k"
        cols="4"
      >
        <v-sheet class="pa-2 ma-2">
          .justify-space-between
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>

```

![https://ithelp.ithome.com.tw/upload/images/20230921/201625428lOYratCGG.png](https://ithelp.ithome.com.tw/upload/images/20230921/201625428lOYratCGG.png)

可以看到其中幾個屬性，比如説在v-col裏面有cols屬性，我先來介紹這個。

在Vuetify的世界裏面，一個v-row會被分成12等分，當你使用v-col的時候，他會自動平均分配每一個v-col，比如説一v-row裏面只放了一個v-col，那這個v-col就會占據12格，而如果你在一個v-row裏面放了4個v-col，那每個v-col就會占據3格，以此類推。而v-col的 cols屬性就是用來描述一個v-col占據多少格。還有一些justify屬性，或是align屬性，這些就是平常在寫的style，有經驗的話一看就會明白怎麽用了，如果不知道那是什麽，可以去看官方的文檔和範例。

好了，今天的内容再説下去就太多了，我們先到這裏爲止。

復習一下今天學習的東西，我們學習了Vuetify的佈局排版，一個根組件必須要有一個v-app，并且我們可以利用v-layout，v-container，v-row，v-col等來進行佈局，并且要注意的是，不論是否有沒有使用v-row和v-col，Vuetify的佈局系統默認就會使網格的（grid），也就是剛剛介紹的一個row裏面會有12格平均分配寬度的格子。并且，我們也學會了使用v-parallax來當作我們的背景圖片，做成一個使影像看起來比視窗滾動得慢的一個背景效果。

本篇終。