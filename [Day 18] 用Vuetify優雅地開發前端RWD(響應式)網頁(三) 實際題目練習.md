# [Day 18] 用Vuetify優雅地開發前端RWD(響應式)網頁(三) 實際題目練習

讓我們繼續昨天的練習，我們這邊可以先想一想，一般來説這樣類型的官網他們的首頁都是一些什麽内容。

象是我們常用的輪播圖，Vuetify也内建給我們了：

# **v-carousel**

v-carousel 是在 v-window 的基礎上進行了擴展，提供了旨在顯示圖像的附加功能。 預設隱藏分隔符 懸停時顯示箭頭等。

```
<template>
  <v-carousel>
<!-- 輪播的內容 --><v-carousel-item v-for="(item, index) in items" :key="index">
<!-- 輪播項的內容 --><v-img :src="item.image" aspect-ratio="1"></v-img>
      <div>{{ item.text }}</div>
    </v-carousel-item>
  </v-carousel>
</template>

```

在上面的例子中，我們使用`<v-carousel>`元素來包裹輪播的內容，並使用`<v-carousel-item>`元素來定義每個輪播項的內容。這只是一個簡單的示例，現在讓我們看看可用的props以自訂輪播圖的行為和外觀。

以下是Vuetify輪播圖組件的一些重要props，你可以使用這些props來自訂輪播圖的外觀和行為：

- cycle（預設值：true）：設置為true以啟用無限循環輪播，當到達最後一個輪播項時，將返回到第一個。
- hide-delimiters（預設值：false）：設置為true以隱藏輪播圖的指示器，通常顯示在輪播圖底部，用戶可以通過它們進行手動切換。
- interval（預設值：6000，以毫秒為單位）：設置輪播項之間的切換時間間隔。
- next-icon 和 prev-icon：設置自訂的圖標來控制下一個和上一個輪播項的切換。
- show-arrows（預設值：true）：設置為true以顯示輪播圖的左右箭頭。
- show-navigation（預設值：true）：設置為true以顯示指示器（通常是小圓點），用戶可以通過它們進行手動切換。
- transition（預設值：''）：設置輪播項切換的過渡效果，例如：'fade'、'slide-x'、'slide-y'等。
- vertical（預設值：false）：設置為true以啟用垂直輪播。

像是我們可以這樣用來製作我們的官網首頁：

```xml
<v-carousel height="700">
    <v-carousel-item cover v-for="(item, index) in carouselItems" :src="item.imageUrl" :key="index"></v-carousel-item>
</v-carousel>

```

# **v-card**

```
<template>
  <v-card>
    <v-card-title>Card Title</v-card-title>
    <v-card-text>
      This is the content of the card. You can put any text or other elements here.
    </v-card-text>
  </v-card>
</template>

```

上面的代碼演示了一個最基本的v-card組件，它包含了標題和內容。現在，讓我們看看可用的props以自訂v-card的外觀和行為。

- color：設置卡片的背景顏色，可以是預定義的顏色名稱，也可以是CSS顏色值。
- dark（預設值：false）：設置為true以應用深色主題樣式。
- elevation（預設值：2）：設置卡片的陰影深度。
- flat（預設值：false）：設置為true以禁用卡片的陰影效果。
- outlined（預設值：false）：設置為true以將卡片呈現為輪廓式外觀。
- raised（預設值：false）：設置為true以提高卡片的陰影深度，使其看起來更突出。
- tile（預設值：false）：設置為true以使卡片的內容充滿整個卡片，而不是在中間對齊。
- max-width：設置卡片的最大寬度，可以是CSS寬度值。
- min-height：設置卡片的最小高度，可以是CSS高度值。
- width：設置卡片的寬度，可以是CSS寬度值。

接下來，我們可以使用v-container來確保我們的内容不會不會觸及到我們的網頁的邊緣（即顯示器的邊緣），因爲這樣的話顯示效果很醜，用戶體驗也不會好，并且搭配我們的v-row和v-col來使用。

這次我們使用v-card這個組件。

```
<v-container>
    <h2>寵物服務</h2>
    <v-row>
        <v-col v-for="(service, index) in petServices" :key="index" cols="12" md="4">
            <v-card height="380">
                <v-img cover :src="service.imageUrl" height="200"></v-img>
                <v-card-title>{{ service.name }}</v-card-title>
                <v-card-text>{{ service.description }}</v-card-text>
                <v-card-actions>
                    <v-btn color="primary">了解更多</v-btn>
                </v-card-actions>
            </v-card>
        </v-col>
    </v-row>
</v-container>

```

我們來看一下完整的代碼和顯示效果是怎麽樣的：

```
<template>
    <v-main class="pt-0">
        <v-carousel height="700">
            <v-carousel-item cover v-for="(item, index) in carouselItems" :src="item.imageUrl" :key="index">
            </v-carousel-item>
        </v-carousel>
        <v-container>
            <h2>寵物服務</h2>
            <v-row>
                <v-col v-for="(service, index) in petServices" :key="index" cols="12" md="4">
                    <v-card height="380">
                        <v-img cover :src="service.imageUrl" height="200"></v-img>
                        <v-card-title>{{ service.name }}</v-card-title>
                        <v-card-text>{{ service.description }}</v-card-text>
                        <v-card-actions>
                            <v-btn color="primary">了解更多</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-col>
            </v-row>
        </v-container>
        <v-container>
            <v-row>
                <v-col cols="12">
                    <h2>精選商品</h2>
                </v-col>
                <v-col v-for="(product, index) in products" :key="index" cols="12" md="4">
                    <v-card height="350">
                        <v-img cover :src="product.imageUrl" height="200"></v-img>
                        <v-card-title>{{ product.name }}</v-card-title>
                        <v-card-text>{{ product.description }}</v-card-text>
                        <v-card-actions>
                            <v-btn color="primary">購買</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-col>
            </v-row>
        </v-container>
    </v-main>
</template>

<script>
export default {
    data() {
        return {
            petServices: [
                {
                    name: "寵物美容",
                    description: "專業的寵物美容服務，讓您的寵物看起來更加美麗和健康。",
                    imageUrl: "https://d3nb97lilvchvx.cloudfront.net/category_page/pet_grooming.jpg",
                },
                {
                    name: "寵物健康檢查",
                    description: "定期的健康檢查，確保您的寵物身體狀況良好。",
                    imageUrl: "https://pet-care.event2.tw/Content/upload/News/294a344a-540d-4802-b2f6-cc0f1953866e.jpg",
                },
                {
                    name: "寵物培訓",
                    description: "訓練您的寵物成為良好的伴侶，學習基本指令和行為。",
                    imageUrl: "https://images.unsplash.com/photo-1658314756284-7a83b8729efb?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80",
                },
            ],
            carouselItems: [
                { imageUrl: "https://images.unsplash.com/photo-1563460716037-460a3ad24ba9?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1974&q=80" },
                { imageUrl: "https://images.unsplash.com/photo-1557495235-340eb888a9fb?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2013&q=80" },
                { imageUrl: "https://images.unsplash.com/photo-1531884070720-875c7622d4c6?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2071&q=80" },
                { imageUrl: "https://images.unsplash.com/photo-1581888227599-779811939961?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2148&q=80" },
                { imageUrl: "https://images.unsplash.com/photo-1494947665470-20322015e3a8?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80" },
                { imageUrl: "https://images.unsplash.com/photo-1497752531616-c3afd9760a11?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80" },
                { imageUrl: "https://images.unsplash.com/photo-1625794084867-8ddd239946b1?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80" },
            ],
            products: [
                {
                    name: "寵物食品",
                    description: "新鮮美味的寵物食品，讓您的寵物健康快樂。",
                    imageUrl: "https://images.unsplash.com/photo-1548767797-d8c844163c4c?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2071&q=80",
                },
                {
                    name: "寵物玩具",
                    description: "各種有趣的寵物玩具，提供娛樂和活力。",
                    imageUrl: "https://images.unsplash.com/photo-1585837575652-267c041d77d4?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2095&q=80",
                },
                {
                    name: "寵物用品",
                    description: "多種寵物用品，滿足您寵物的所有需求。",
                    imageUrl: "https://images.unsplash.com/photo-1520087619250-584c0cbd35e8?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2094&q=80",
                },
            ],
        };
    },
};
</script>

<style scoped>
/* 在這裡添加自定義的CSS樣式 */
</style>

```

![https://ithelp.ithome.com.tw/upload/images/20230923/20162542JfgzJjBfZN.png](https://ithelp.ithome.com.tw/upload/images/20230923/20162542JfgzJjBfZN.png)

好了，今天就到這裏。我們今天使用了幾個簡單的排版概念，就可以輕鬆地完成一些官網的首頁内容，明天開始會陸續完善其他頁面。

本篇終。