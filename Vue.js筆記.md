## 概念
View : 透過 HTML 及模板語法渲染出來的畫面。  
View Model : 使用 Vue.js 所建立起來的實體。  
Model : 後端程式中的商業邏輯。  
## 引入
一般的專案中通常都是使用 Webpack 來引入，但也可以使用 <script> 元素來引入。

    <!DOCTYPE html>
    <html>
    <head>
        ...
        <script src="https://unpkg.com/vue"></script>
    </head>
    <body>
        ...
    </body>
    </html>
## 建立實體

    <script>
        var vm = new Vue({
            ...
        });
        ...
    </script>
## 第一個例子

    var vm = new Vue({
        el: '#app',
        data: {
            message: "This is local data.",
        },
        methods: {
            getRemoteMessage() {
                Promise.resolve("Get remote data.")
                    .then((res) => {
                        this.message = res;
                    });
            },
        },
    });
這樣可以將vue實例綁定在<code>id</code>為<code>app</code>的元素上。  
## 取得實例中的變數
    var vm = new Vue({
      ...
      data: {
        a: 1
      }
    });

    console.log(vm.$data.a); // 1
像是<code>$data</code>這種有前綴<code>$</code>的屬性是 Vue 實例所配置的，有前綴是為了跟使用者定義的屬性作區別，所有的<code>$</code>屬性可以在 API 文件中找到。  
直接對修改<code>$data</code>，Vue就會重新渲染畫面造成變化:

    vm.$data.a = 2; // equal to vm.a = 2
## 監聽資料變化
    var vm = new Vue({
      ...
      data: {
        a: 1,
        b: 1
      },
      ...
    });

    ...
    vm.$watch('a', function (newValue, oldValue) {
      this.b = oldValue;
    })
<code>$watch</code>: 在第一個參數中設定的資料變化時觸發第二個參數的函式，在這個例子中當<code>a</code>發生變化時，會將未修改時的<code>a</code>數值設給<code>b</code>。
## 生命週期鉤子
<img src="https://d1dwq032kyr03c.cloudfront.net/upload/images/20181019/20107789jljtZxzJPJ.png" width=300px>
## Mustache 標籤
其實就是雙層大括號<code>{{}}</code>。  
每次變數值有變動都會re-render。  
Instance:

    var vm = new Vue({
      el: "#app",
      data: {
        a: 1
      },
      created() {
        setInterval(() => {
          this.a++;
        }, 1000);
      }
    }); 
template:

    <div id="app">
      {{a}}
    </div>
這樣的話，讀值每秒都會更新一次。  
但若加上 v-once 這個 directive 可以讓 Mustache 標籤只渲染一次，使用的方式如下:

    <div id="app">
      <span>{{a}}</span>
      <span v-once>render once: {{a}}</span>
      ...
    </div>
也可以塞Javascript陳述式：  
    
    <div id="app">
      ...
      <div>plus one: {{a + 1}}</div>
      <div>ternary expressions: {{a % 2 === 0 ? 'even' : 'odd'}}</div>  
      <div>length: {{a.toString().length}}</div>
      <div>Math.pow2: {{Math.pow(a, 2)}}</div>
    </div>
  ## Directives
<code>v-html</code>會將資料當作 HTML 做渲染  
使用<code>v-if</code>來決定是否要渲染元素  
要綁定實例中變數的話，要用<code>v-bind</code>，例如：

    <div id="app">
      ...
      <div v-bind:id="id">Bind Directives</div>
    </div>
<code>v-bind</code>和<code>v-on</code>實在太常用了，所以都有縮寫！

    <!-- 一般寫法 -->
    <button v-bind:disabled='isDisabled'>I am disabled</button>
    <!-- 簡寫 -->
    <button :disabled='isDisabled'>I am disabled</button>
<code>v-bind</code>使用<code>:</code>當作簡寫。

    <!-- 一般寫法 -->
    <button v-on:click.once='click'>b--</button>
    <!-- 簡寫 -->
    <button @click.once='click'>b--</button>
<code>v-on</code>使用<code>@</code>當作簡寫。
## 改寫表達式的技巧：計算屬性
    var vm = new Vue({
      el: '#app',
      data: {
        number: 1
      },
      computed: {
        numberEvenOrOdd() {
          return this.number % 2 === 0 ? 'even' : 'odd';
        }
      }
    });

    <div id="app">
      <button @click="number++">+</button>
      <button @click="number--">-</button>
      <div>
        <span>Number {{number}} is {{numberEvenOrOdd}}</span>
      </div>
    </div>
要注意的是，<code>compute</code>和<code>method</code>不一樣哦！

    var vm = new Vue({
      ...
      computed: {
        ...
        datePlusNumberComputed() {
          return Date.now();
        }
      },
      methods: {
        ...
        datePlusNumberMethod() {
          return Date.now();
        }
      }
    });

    <div id="app">
      ...
      <div>
        <div>Computed: {{datePlusNumberComputed}}</div>
        <div>Method: {{datePlusNumberMethod()}}</div>
      </div>
    </div>
計算屬性算完之後就不會執行Function，因此值不會改變。  
方法就不同了，只要每次重新渲染畫面就會執行一次。  
如果這個回傳值跟資料來源的變化有關，那應該在來源有變化時在執行即可，否則會產生不必要的運算時間，降低效能，所以當要取得某個結果跟其他資料有關的值的話，用計算屬性才是上策。
## 計算屬性與監聽
兩者有相似的功能和使用情境，計算屬性會在有關的資料產生變化時觸發 callback 函數更新屬性值，而監聽器則是以監聽單個資料變化為主，當監聽的資料產生變化時會觸發 callback 函數，執行後續的處理。  
https://ithelp.ithome.com.tw/articles/10204091
## Class 的綁定
這樣就可以：

    <div id="app">
      <span>
        <span :class="[arrColor, 'bold']">Array Class</span>
      </div>
    </div>

    var vm = new Vue({
      el: '#app',
      data: {
        arrColor: 'red'
      }
    });
方便，不過只有 Class 及 Style 支援這個特殊的轉換。
可以把物件放進計算屬性或是陣列。  
https://ithelp.ithome.com.tw/articles/10204949
## 條件渲染
<code>v-if</code>, <code>v-else-if</code>, <code>v-else</code>, and <code>v-show</code>  
https://ithelp.ithome.com.tw/articles/10205764
## 列表渲染
<code>v-for</code>  

    var vm = new Vue({
      data: {
        itemHeader: 'number',
        items: ['one', 'two', 'three']
      }
    });

    <div v-for="item in items">{{itemHeader}} is {{item}}</div>var vm = new Vue({
陣列裡也可以是複雜物件：
    
    data: {
        itemHeader: 'number',
        items: ['one', 'two', 'three'],
        objItems: [
          {name: 'one', number: 1},
          {name: 'two', number: 2},
          {name: 'three', number: 3}
        ]
      }
    });

    建立一個 objItems ，每個元素都是一個有 name 及 number 的物件。

    <div id="app">
      <div v-for="item in items">{{itemHeader}} is {{item}}</div>
      <div v-for="item in objItems">Eng: {{item.name}}, Number: {{item.number}}</div>
    </div>
## 響應系統
<img src="https://d1dwq032kyr03c.cloudfront.net/upload/images/20181029/201077897h3nQ3gUqy.png" width=300px>  
建立實體時會將選項物件中定義資料屬性都設上 getter 及 setter ，並將每個資料的初始值丟給渲染函數去建立Virtual DOM Tree。
建立實體後才加入的屬性因為沒有被給予 getter 及 setter ，所以不會被響應系統察覺。

## 事件處理 
使用 JavaScript 陳述式: 這樣的設定方式適合簡單的事件處理。  
使用方法名稱: 在選項物件中設定 methods 屬性，可以叫用對應的方法，而傳入的第一個參數為原生的 DOM 事件物件。  
使用 JavaScript 陳述式叫用方法: 可以傳入自定義參數，如果要使用原生 DOM 事件物件可以用 $event 傳入方法中。  
使用物件定義: 物件可以設定多個方法，但限制是只能使用方法名稱設置，並且不能使用修飾符。  
只使用事件修飾符: 只執行事件修飾符中設定的代碼。  
### 事件修飾符
<code>.stop</code> : 停止觸發上層 DOM 元素事件。  
<code>.prevent</code> : 避免瀏覽器預設行為。  
<code>.capture</code> : 不管觸發事件的目標是否是下層， 設定 capture 的事件一定會先觸發。  
<code>.self</code> : 只有觸發此 DOM 元素本身才會觸發 self 事件。  
<code>.once</code> : 此事件只觸發一次。  
<code>.passive</code> : 無視 prevent 功能。  
## 表單綁定
<code>v-bind</code>及 <code>{{}}</code>綁定資料至模板上都是從view model到view的單向綁定。
模板上如果有 input 或是 textarea 等的輸入欄位時，會需要將在 view 上更新的資料傳回至 view model 上，這時就需要使用 v-model 這個雙向綁定的屬性。
### 單行字串
    <button @click="msg=''">Clear</button>
    <input placeholder="Edit" v-model="msg"> {{msg}}
### 多行字串
    <textarea placeholder="Edit" v-model="msgarea"></textarea>
    <p style="white-space: pre-line">{{msgarea}}</p>/
### 下拉式選單
    <div>
      <div>What are you select? {{selected}}</div>
      <select v-model="selected">
        <option disabled value="">Please select one</option>
        <option value="A">A</option>
        <option value="B">B</option>
        <option value="C">C</option>
      </select>
      <br/>
    </div>
### 修飾符
<code>.lazy</code>: 更新的時間點會被延到<code>change</code>事件時。  
<code>.number</code>: 讓此輸入框的類型維持在數字。
<code>.trim</code>: 會trim使用者輸入的字串。
## 組件(Component)
根節點是生成<code>new Vue</code>的實體。  
底下組件的建立是使用<code>Vue.component</code>註冊。  
只有<code>new Vue</code>可以使用<code>el</code>屬性定義掛載目標(因為它是根節點)。  
所以組件需要使用<code>template</code>或是<code>render</code>函數設定目標模板，如下：  

    Vue.component('button-counter', {
      data: function() {
        return {
          count: 0
        }
      },
      template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
    })
在模板則是這樣使用：

    <div id="app">
      <button-counter></button-counter>
    </div>
### <code>data</code>屬性
由於組件可能會有復用的情形，因此每個資料屬性必須要擁有獨立的實體，所以在組件中的<code>data</code>屬性需要使用函數來回傳一個全新的物件。
### <code>is</code>屬性
#### 動態載入組件
如果此組件會在程式執行時改變，那就不能直接設定在模板上，可以使用<code>is</code>取得組件，Vue.js會依照這個組件去渲染DOM元素，如下例所示:

    <div id="app3">
      <button @click="dynamicComponent='hello'">Hello</button>
      <button @click="dynamicComponent='bye'">Bye</button>
      <button @click="dynamicComponent={template: `<p style='color: purple'>Good</p>`}">Good</button>
      <component :is="dynamicComponent"></component>
    </div>
#### HTML 元素配置限制

在像是 <ul> 、<ol> 、 <table> 及 <select> 標籤下會有限制使用的元素，例如 <table> 下層就一定要使用 <tr> ，可是當你使用組件設定這些下層元素時，會如下面這樣設定:

    <table id="app2">
      <my-row></my-row>
    </table>

這時 會因為是錯誤的標籤而被抬升，<code><my-row></code>被抬到<code><table></code>外面了，  
為了防止這樣的問題，我們可以用 is 屬性在 <tr> 標籤上設定想要使用的組件，這樣就不會被判定為錯誤的標籤了:
    <table id="app2">
      <tr is="my-row"></tr>
    </table>

#### 組件內容
如果想要設定組件內容的話，可以使用 <slot> 標籤來決定組件內容的配置:
    
    Vue.component('button-counter', {
      data: function() {
        return {
          count: 0
        }
      },
      template: `
        <div>
          <p>{{count}}</p>
          <button v-on:click="count++">
            <slot>
              Click
            </slot>
          </button>
        </div>
      `
    });
我們將原本的按鈕內容中加上<code><slot></code>標籤，當我們要客製按鈕上的字串時，只要像下面這樣:

    <div id="app4">
      <button-counter>Hello Click Button</button-counter>
    </div>



