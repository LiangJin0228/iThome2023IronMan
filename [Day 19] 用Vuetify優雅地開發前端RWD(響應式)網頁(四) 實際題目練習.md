# [Day 19] 用Vuetify優雅地開發前端RWD(響應式)網頁(四) 實際題目練習

我們在之前所作出來的首頁只有畫面而沒有功能，我們今天就來完成它的一些功能的部分。

首先我們先來改一下它的index.html`<title>小明寵物店</title>`，然後在router的部分來改一下路徑:

```tsx
// Composablesimport { createRouter, createWebHistory } from "vue-router";

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
];

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
});

export default router;

```

我們之前做的`<v-text-field prepend-icon="mdi-magnify" single-line></v-text-field>`這個欄位我們暫時用不到，將它拿掉，然後換上我們要的導航到不同的頁面。

這裏我們可以使用：

# **v-btn**

以下是一些常用的 v-btn 屬性（props）：

- to：用於指定路由導航的目標路徑，當按鈕被點擊時，將導航到這個路徑。
- href：用於指定外部連結的 URL。當設置 href 時，按鈕將作為超連結使用。
- color：用於設置按鈕的顏色，可以是預定義的顏色主題（例如："primary"、"success"、"error"）或自定義的顏色。
- outlined：一個布林值，當設置為 true 時，按鈕將呈現為輪廓按鈕，而不是實心按鈕。
- text：一個布林值，當設置為 true 時，按鈕將呈現為文本按鈕，沒有背景顏色和邊框。
- disabled：一個布林值，當設置為 true 時，按鈕將禁用，無法點擊。
- block：一個布林值，當設置為 true 時，按鈕將擴展到其容器的寬度。
- icon：用於添加圖標到按鈕內容的屬性。

這些是一些常見的 v-btn props，你可以根據你的需求進一步查閱 Vuetify 官方文檔以了解更多屬性和選項，並根據你的項目需求進行自定義。

比如説，我想要在我的網站除了首頁之外，導航到購物頁面，關於我們頁面，聯係我們頁面，那我可以使用：

```
<v-btn color="white" variant="text" class="mx-2" rounded="xl" to="/index">首頁</v-btn>
<v-btn color="white" variant="text" class="mx-2" rounded="xl" to="/shop">購物</v-btn>
<v-btn color="white" variant="text" class="mx-2" rounded="xl" to="/about">關於我們</v-btn>
<v-btn color="white" variant="text" class="mx-2" rounded="xl" to="/contact">聯係我們</v-btn>

```

這個時候有導航按鈕了，我們只需要再到router.js簡單注冊一下：

```css
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

```

我們可以先從簡單的頁面開始，比如説，`關於我們`的頁面：

```xml
<template>
    <v-main>
<!-- 公司簡介部分 --><section>
            <v-container class="text-center">
                <h1 class="mb-4 text-h1">小明寵物店</h1>
                <v-divider></v-divider>

<!-- 公司介紹 --><section class="my-4">
                    <h4 class="text-h4">公司介紹</h4>
                    <p>
                        小明寵物店成立於2000年，致力於提供最優質的寵物服務和產品。我們以熱情的態度、高品質的服務和公道的價格而自豪。
                    </p>
                </section>

<!-- 企業文化 --><section class="my-4">
                    <h4 class="text-h4">企業文化</h4>
                    <v-list>
                        <v-list-item>
                            <v-list-item-content>
                                <v-list-item-title class="text-body-1">關懷與尊重：我們對待寵物和寵物主人都充滿關懷和尊重。</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                        <v-list-item>
                            <v-list-item-content>
                                <v-list-item-title
                                    class="text-body-1">卓越的服務：我們追求卓越，不僅提供高品質的寵物產品，還為寵物主人提供專業的咨詢和支持。</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                        <v-list-item>
                            <v-list-item-content>
                                <v-list-item-title class="text-body-1">社區參與：我們驕傲地融入社區，並積極參與寵物相關的公益活動。</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                    </v-list>
                </section>

<!-- 公司願景 --><section class="my-4">
                    <h4 class="text-h4">公司願景</h4>
                    <p>
                        小明寵物店的願景是成為寵物服務領域的全球領袖。我們希望通過我們的努力，改善寵物和寵物主人的生活，並為社會創造更多的善意和關懷。
                    </p>
                </section>

<!-- 公司賣點 --><section class="my-4">
                    <h4 class="text-h4">我們的賣點</h4>
                    <v-list>
                        <v-list-item>
                            <v-list-item-content>
                                <v-list-item-title class="text-body-1">專業團隊：我們擁有一支經過專業培訓的團隊，了解不同種類的寵物需求。</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                        <v-list-item>
                            <v-list-item-content>
                                <v-list-item-title class="text-body-1">品質保證：我們只提供最高品質的寵物產品，包括食品、用具、藥品等。</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                        <v-list-item>
                            <v-list-item-content>
                                <v-list-item-title class="text-body-1">定制服務：我們提供定制的服務和建議，以滿足不同寵物的需求。</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                        <v-list-item>
                            <v-list-item-content>
                                <v-list-item-title class="text-body-1">社區參與：我們積極參與社區，定期舉辦寵物相關的活動和義工工作。</v-list-item-title>
                            </v-list-item-content>
                        </v-list-item>
                    </v-list>
                </section>
            </v-container>
        </section>

<!-- 服務部分 --><section>
            <v-container>
                <h2>我們的服務</h2>
                <v-row>
                    <v-col cols="12" md="4">
                        <v-card>
                            <v-img src="/assets/grooming.jpg" alt="寵物美容服務"></v-img>
                            <v-card-title>寵物美容</v-card-title>
                            <v-card-text>
                                我們提供專業的寵物美容服務，讓您的寵物看起來煥然一新。
                            </v-card-text>
                        </v-card>
                    </v-col>
                    <v-col cols="12" md="4">
                        <v-card>
                            <v-img src="/assets/boarding.jpg" alt="寵物寄養服務"></v-img>
                            <v-card-title>寵物寄養</v-card-title>
                            <v-card-text>
                                無論您需要短期或長期的寵物寄養，我們都為您提供安全舒適的環境。
                            </v-card-text>
                        </v-card>
                    </v-col>
                    <v-col cols="12" md="4">
                        <v-card>
                            <v-img src="/assets/training.jpg" alt="寵物訓練服務"></v-img>
                            <v-card-title>寵物訓練</v-card-title>
                            <v-card-text>
                                我們的專業訓練師將幫助您的寵物培養好行為和技能。
                            </v-card-text>
                        </v-card>
                    </v-col>
                </v-row>
            </v-container>
        </section>

<!-- 產品部分 --><section>
            <v-container>
                <h2>我們的產品</h2>
                <v-row>
                    <v-col cols="12" md="4">
                        <v-card>
                            <v-img src="/assets/pet-food.jpg" alt="寵物食品"></v-img>
                            <v-card-title>寵物食品</v-card-title>
                            <v-card-text>
                                從高質量的寵物食品到零食，我們有各種各樣的選擇。
                            </v-card-text>
                        </v-card>
                    </v-col>
                    <v-col cols="12" md="4">
                        <v-card>
                            <v-img src="/assets/toys.jpg" alt="寵物玩具"></v-img>
                            <v-card-title>寵物玩具</v-card-title>
                            <v-card-text>
                                我們提供各種各樣的寵物玩具，讓您的寵物保持活躍和快樂。
                            </v-card-text>
                        </v-card>
                    </v-col>
                    <v-col cols="12" md="4">
                        <v-card>
                            <v-img src="/assets/accessories.jpg" alt="寵物用品"></v-img>
                            <v-card-title>寵物用品</v-card-title>
                            <v-card-text>
                                從項圈到穿戴配件，我們提供一系列寵物用品以滿足您的需求。
                            </v-card-text>
                        </v-card>
                    </v-col>
                </v-row>
            </v-container>
        </section>

<!-- 聯系信息部分 --><section>
            <v-container>
                <h2>聯系我們</h2>
                <v-row>
                    <v-col cols="12" md="6">
                        <v-card>
                            <v-card-title>營業時間</v-card-title>
                            <v-card-text>
                                周一至周五: 9:00 AM - 6:00 PM<br>
                                周六至周日: 10:00 AM - 4:00 PM
                            </v-card-text>
                        </v-card>
                    </v-col>
                    <v-col cols="12" md="6">
                        <v-card>
                            <v-card-title>聯系信息</v-card-title>
                            <v-card-text>
                                地址：123 寵物大道，城市，郵編<br>
                                電話：(123) 456-7890<br>
                                電子郵件：info@xiaomingpets.com
                            </v-card-text>
                        </v-card>
                    </v-col>
                </v-row>
            </v-container>
        </section>
    </v-main>
</template>

```

# **效果展示**

![https://ithelp.ithome.com.tw/upload/images/20230924/2016254261ZJoO35sk.png](https://ithelp.ithome.com.tw/upload/images/20230924/2016254261ZJoO35sk.png)

再來就是`聯係我們`的頁面，這次我想要教各位如何使用v-form來快速建立一些表單：

```xml
<v-form>
    <v-autocomplete
      v-model="selected"
      :items="items"
      chips
      hide-details
      hide-no-data
      hide-selected
      label="To"
      multiple
      single-line
    ></v-autocomplete>

    <v-divider></v-divider>

    <v-text-field
      v-model="subject"
      hide-details
      label="Subject"
      single-line
    ></v-text-field>

    <v-divider></v-divider>

    <v-textarea
      v-model="title"
      counter
      label="Message"
      maxlength="120"
      single-line
    ></v-textarea>
</v-form>

```

# **效果展示**

![https://ithelp.ithome.com.tw/upload/images/20230924/20162542aJqlNgMNEw.png](https://ithelp.ithome.com.tw/upload/images/20230924/20162542aJqlNgMNEw.png)

那我們今天基本上靜態頁面大的部分都寫完了，接下來我們要去實現購物網站的頁面了，大家可以期待一下接下來的實作聯係。

本篇終。