# [Day 10] Vue 3 Model => {method, watch}

# **methods（方法）**

在Vue 3中，methods是一個包含了各種自定義方法的對象，你可以在Vue組件中定義這些方法。這些方法可以執行各種不同的操作，例如處理用戶輸入、數據處理、網絡請求等。以下是如何定義和使用methods的示範：

```xml
<template>
  <div>
    <button @click="sayHello">點我</button>
  </div>
</template>

<script>
export default {
  methods: {
    sayHello() {
      alert('Hello World');
    }
  }
}
</script>

```

在這個例子中，我們定義了一個名為sayHello的方法，當用戶點擊按鈕（`@click`）時，該方法會彈出一個包含"Hello World"的警告框。你可以定義很多個method，然後在template中調用它們。

這樣的事情我們會叫做事件的處理，點擊是一個事件，鼠標移到某個DOM元素上（hover）也可以是一個事件，按下鍵盤的任何一個key也可以是事件。當我們談論事件時，通常是指在網頁或應用程序中發生的某種特定動作或交互，這些動作可以被JavaScript代碼捕獲和處理。不只是user主動觸發事件，事件也可以是由系統或應用程序自動生成的，例如定時器（timer）觸發的事件，或是在app的生命周期鈎子（lifecycle hooks）裏觸發事件。

以下是一些關於事件的詳細解釋：

- 事件的種類： 事件可以分為多種類型，包括鼠標事件（例如點擊、懸停、拖動）、鍵盤事件（例如按鍵按下、釋放、鍵入字符）、表單事件（例如提交、更改、重置）、窗口事件（例如調整大小、滾動、關閉）等等。不同的事件類型對應不同的交互行為。
- 事件目標： 每個事件都與一個事件目標（通常是HTML元素）相關聯。事件目標是觸發事件的元素，你可以通過JavaScript代碼選擇和操作這個元素。例如，當用戶點擊一個按鈕時，該按鈕元素就成為事件目標。
- 事件監聽器： 為了捕獲和處理事件，我們可以添加事件監聽器（就像是原生JS的EventListener）到特定的元素上。事件監聽器是一段JavaScript代碼，當特定事件在元素上觸發時，這段代碼就會執行。例如，我們可以通過事件監聽器在按鈕上監聽點擊事件，並在觸發時執行相應的操作。
- 事件對象： 每個事件處理程序都接收一個事件對象作為參數（如下例子：）

```
//這個event就是事件對象
changeScrollDirections(event) {
  event.preventDefault();
  const delta = event.deltaY;
  const scrollContainer = this.$refs.info.$el;// 使用 $el 來獲取 DOM 元素if (scrollContainer) {
    scrollContainer.scrollBy({
      left: delta,
    });
  }
},

```

該對象包含了有關事件的詳細信息，例如事件類型、事件目標、滑鼠位置等。開發人員可以使用這個對象來瞭解事件的上下文和屬性。

- 事件傳播： 事件在DOM中具有冒泡和捕獲兩種傳播方式（先不解釋，之後再填坑）。你可以選擇性地使用事件捕獲或事件冒泡來處理事件。

總之，事件是Web開發中的重要概念，它們使我們能夠對用戶的操作和瀏覽器的行為作出反應，實現交互性和動態性的網頁和應用程序。

所以總結一句話，你要處理事件，你就用method將這個事件的處理方式包裝起來，並給它一個名字。

# **watch（監視）**

watch選項用於監視數據的變化，當特定數據的值發生變化時，你可以執行自定義的操作。這在處理非同步數據或需要特殊處理的數據時非常有用。以下是一個watch的示例：

（和我在GitHub上寫的不一樣，這個是比較嚴謹的寫法）

```jsx
const app = Vue.createAPP({
	data() {
		return {
			arr: [
				"Hello",
				"這個是一個",
				"測試"
			],
			count: 4
		}
	},
	watch: {
		arr: {
			handler(newValue){
				this.count = newValue.length;
			}
			deep: true
		}
	}
});
app.mount('#app');

```

關於deep這個屬性，當你watch的東西如果你將他視爲一個整體，那可以不使用deep，但是如果你關心的是這個物件裏面任何一個屬性若是發生變化，你的行爲都要改變的話，那就用deep寫法

理解`watch`中的`deep`選項的實際用例可能會更清晰。讓我們看一個實際的例子。

假設有一個Vue組件，其中有一個名為`userData`的對象，該對象包含用戶的詳細信息。你想要監視此對象的變化並在其中任何屬性發生更改時觸發一些操作。如果不使用`deep`選項，只有當`userData`的引用更改時，`watch`才會觸發。這意味著如果更改了`userData`對象內部的屬性，`watch`將不會觸發。

```xml
<template>
  <div>
    <button @click="changeUserData">Change UserData</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userData: {
        name: 'John',
        age: 30,
      },
    };
  },
  watch: {
    userData: {
      handler(newVal, oldVal) {
        console.log('userData changed');
      },
      deep: false,// 默認值是 false，可以省略
    },
  },
  methods: {
    changeUserData() {
// 這里只是修改userData對象內部的屬性，而不是整個對象的引用this.userData.name = 'Jane';
      this.userData.age = 25;
    },
  },
};
</script>

```

在上面的範例中，如果點擊"Change UserData"按鈕，`userData`對象內部的屬性被修改，但是`watch`並不會觸發，因為`deep`默認為`false`，Vue只會監聽對象的引用變化。

如果想要深度監聽對象內部屬性的變化，需要將`deep`選項設置為`true`：

```jsx
watch: {
  userData: {
    handler(newVal, oldVal) {
      console.log('userData changed');
    },
    deep: true,
  },
},

```

這樣，在`userData`對象內部的屬性發生變化時，`watch`會觸發。

所以，`deep`選項的使用取決於是否需要深度監聽對象內部屬性的變化，而不僅僅是對象的引用變化。

今天比較忙，沒辦法跟各位多説，有些可能説不明白的明天會再和各位補充，敬請見諒。明天除了補充説明，還會講解生命周期的實際運用，各位可以期待。

本篇終。