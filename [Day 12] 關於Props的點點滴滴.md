# [Day 12] 關於Props的點點滴滴

# **什麽是props？**

Props是Vue框架中用於在父組件傳遞數據到子組件的一種機制。它允許你將數據從一個組件傳遞到另一個組件，以便在子組件中使用這些數據。

# **為什麽我們需要props？**

在前端開發中，通常會有一些數據需要在不同的組件之間共享或傳遞。而props是一種非常有效的方式來實現這種數據傳遞。它幫助我們構建了一個單向數據流，從父組件向子組件傳遞數據，這使得我們的組件可以更加模塊化和可覆用。

# **如何使用props？**

首先，我們需要在父組件中定義props並將數據傳遞給子組件。在Vue中，props是子組件的屬性，我們可以將這些屬性定義為一個對象，例如：

我們平時在寫html的時候，會爲一些標簽加上屬性，如input可以放`type`屬性來決定輸入的是什麽類型的内容，或是`<a>`標簽的href屬性決定跳轉的網頁鏈接。

同樣，我們在製作了一個component之後，我們也可以為這個component放上屬性，在Vue裏面，這個東西叫做props。并且要注意的是，這個props是父層傳給子層，而不能子層傳給父層，如果要子層傳給父層，那就需要使用到emit。

我們直接來看步驟：

1. 定義 props： 在子組件中，你可以通過在組件選項中定義 props 屬性來聲明你希望接受的數據。這可以是一個字符串數組，其中包含你希望接受的 prop 名稱。
    
    ```xml
    <script setup>
    const props = defineProps(['greeting-message'])
    
    console.log(props.foo)
    </script>
    
    ```
    
2. 父組件傳遞 props： 在父組件中，你可以使用子組件的自定義標籤來傳遞 props。在標籤中使用与 props 名稱相同的屬性，並將其值設置為你要傳遞的數據。`<MyComponent greeting-message="hello" />`除了向上面這樣的靜態（`<BlogPost title="My journey with Vue" />`）,我們還可以動態的去綁定props，可以使用v-bind的方式：
    
    ```xml
    <!-- 根據一個變量的值動態傳入 --><BlogPost :title="post.title" />
    
    <!-- 根據一個更覆雜表達式的值動態傳入 --><BlogPost :title="post.title + ' by ' + post.author.name" />
    
    ```
    
    并且，在使用props的時候，除了string之外，哪怕是靜態的使用props，我們依然會需要使用到v-bind，比如：
    
    ```ruby
    <BlogPost :likes="42" />
    <BlogPost :is-published="false" />
    <BlogPost :comment-ids="[234, 266, 273]" />
    <BlogPost
      :author="{
        name: 'Veronica',
        company: 'Veridian Dynamics'
      }"
     />
    
    ```
    
    會這樣是因爲，我們的props名稱後面放`=""`,這樣會變成是string（因為這是一個 JavaScript 表達式而不是一個字符串）
    
    另外，像是如果你定義了一個物件，想要將整個物件當成props傳入，你可以直接使用v-bind而不用使用名稱：
    
    ```
    const post = {
      id: 1,
      title: 'My Journey with Vue'
    }
    
    ```
    
    `<BlogPost v-bind="post" />`
    
    不過使用這樣的方式的前提是，你需要保證物件(以上面的)
    
3. 子組件中使用 props： 在子組件中，你可以使用 props 來訪問父組件傳遞的數據。
    
    ```
    <template>
      <div>
        <p>{{ message }}</p>
      </div>
    </template>
    
    <script>
    export default {
      props: ['message']
    }
    </script>
    
    ```
    
4. 驗證 props： 你可以使用 props 對傳遞的數據進行驗證，以確保它們符合預期的類型或其他條件。根據官網的示範：
    
    ```tsx
    defineProps({
    // 基礎類型檢查// （給出 `null` 和 `undefined` 值則會跳過任何類型檢查）
      propA: Number,
    // 多種可能的類型
      propB: [String, Number],
    // 必傳，且為 String 類型
      propC: {
        type: String,
        required: true
      },
    // Number 類型的默認值
      propD: {
        type: Number,
        default: 100
      },
    // 對象類型的默認值
      propE: {
        type: Object,
    // 對象或數組的默認值// 必須從一個工廠函數返回。// 該函數接收組件所接收到的原始 prop 作為參數。default(rawProps) {
          return { message: 'hello' }
        }
      },
    // 自定義類型校驗函數
      propF: {
        validator(value) {
    // The value must match one of these stringsreturn ['success', 'warning', 'danger'].includes(value)
        }
      },
    // 函數類型的默認值
      propG: {
        type: Function,
    // 不像對象或數組的默認，這不是一個// 工廠函數。這會是一個用來作為默認值的函數default() {
          return 'Default function'
        }
      }
    })
    
    ```
    

今天就先簡單介紹到這樣，我們明天見。

本篇終。