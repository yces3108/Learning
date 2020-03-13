# 資料來源
本篇的資訊來源大多來自：  
https://ithelp.ithome.com.tw/users/20107789/ironman/1710
## Vue cli指令入門
https://cli.vuejs.org/zh/guide/installation.html
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
<code>v-for</code>有個特殊屬性，叫做<code>:key</code>，主要的目的是為了避免迴圈物件被不正確的重用，所以，當你有一組清單需要使用迴圈顯示時，請務必確保指定了唯一的<code>:key</code>，以避免渲染出來的元件不正確的情況。  

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
模板上如果有 input 或是 textarea 等的輸入欄位時，會需要將在 view 上更新的資料傳回至 view model 上，這時就需要使用<code>v-model</code>這個雙向綁定的屬性。
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

在像是ul、ol、table及select標籤下會有限制使用的元素，例如table下層就一定要使用tr，可是當你使用組件設定這些下層元素時，會如下面這樣設定:

    <table id="app2">
      <my-row></my-row>
    </table>

這時會因為是錯誤的標籤而被抬升，<code>my-row</code>被抬到<code>table</code>外面了，  
為了防止這樣的問題，我們可以用<code>is</code>屬性在tr標籤上設定想要使用的組件，這樣就不會被判定為錯誤的標籤了:
    <table id="app2">
      <tr is="my-row"></tr>
    </table>

#### 組件內容
如果想要設定組件內容的話，可以使用slot標籤來決定組件內容的配置:
    
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
我們將原本的按鈕內容中加上<code>slot</code>標籤，當我們要客製按鈕上的字串時，只要像下面這樣:

    <div id="app4">
      <button-counter>Hello Click Button</button-counter>
    </div>
## 組件間的資料傳輸 
子組件只要在<code>props</code>屬性加上需要由父組件傳入的參數定義，父組件就可以在配置子組件的時候給予子組件所需的參數屬性，而如果子組件因為變動需要通知父組件，就可以使用<code>$emit</code>通知父組件，而父組件只要設定相關事件的處理函數就可以接收到子組件的資料。
### 父組件使用屬性傳資料給子組件
在子組件使用<code>props</code>屬性註冊客製屬性，由父組件設定在子組件的屬性會變為實體中的屬性

    Vue.component('button-counter', {
      template: `
        <button @click="count+=1">
          {{buttonName}} {{count}} times
        </button>
      `,
      props: ['buttonName'],
      data: function() {
        return {
          count: 0,
        };
      },
    });
在<code>props</code>中設定<code>buttonName</code>，表示<code>buttonName</code>是一個客製屬性，會由父組件在設定模板時給予它的值。
接著設定父組件:  

    <div id="app">
      <button-counter button-name="Click me"></button-counter>
    </div>

    var vm = new Vue({
      el: '#app',
    });
超級好的懶人包！資料來源：  
https://jeremysu0131.github.io/Vue-js-%E7%88%B6%E5%AD%90%E7%B5%84%E4%BB%B6%E6%BA%9D%E9%80%9A-emit-on/
父組件：

    <template>
      <div id="app">
        <child v-on:childMethod="parentMethod"></child>
      </div>
    </template>

    <script>
    import Child from './components/Child';
    export default {
      name: 'App',
      components: {
        Child,
      },
      methods: {
        parentMethod() {
          console.log('Hello World');
        },
      },
    };
    </script>
子組件：

    <template>
      <button @click="handleClick">Emit</button>
    </template>

    <script>
    export default {
      methods: {
        handleClick() {
          this.$emit('childMethod');
        },
      },
    };
    </script>
### 使用<code>$emit</code>反應子組件中的變化
當事件或是監聽器觸發時，子組件可以用<code>$emit</code>方法將變化反應給父組件知道。

    Vue.component('button-counter', {
      template: `
        <button @click="clickPlus">
          {{buttonName}} {{count}} times
        </button>
      `,
      props: ['buttonName'],
      data: function() {
        return {
          count: 0,
        };
      },
    methods: {
      clickPlus: function() {
        this.count += 1;
        this.$emit('click-plus', this.count);
      }
    }
    });
接著只要在父組件中的<code>button-counter</code>中設定<code>click-plus</code>事件的綁定就可以監看此事件:
接著在父組件中使用<code>$event</code>當作傳回的資料做處理:

<button-counter button-name="Click me" @click-plus="count=$event"></button-counter>
## 組件註冊
### 全域註冊
在註冊全域組件時要給予兩個參數: 組件名稱及選項物件:

    Vue.component('component-a', {
      // options
      template: `
        <div>a</div>
      `
    });
這樣一來我們就可以之後的任何實體中使用這個組件，
不只是<code>new Vue</code>實體可以使用，連其他組件也可以使用:
### 區域註冊
全域註冊會將原本不需要的組件也載入進來，拖慢載入的時間。
所以針對某些特定實體設計的組件就可以用區域註冊的方式，註冊在需要它的組件中。
區域註冊會是一個選項物件:

    const componentC = {
      // options
      template: `
        <div>c</div>
      `
    };
這個物件可以由<code>components</code>這個選項物件屬性載入實體內:

var vm = new Vue({
  el: '#app',
  components: {
    'component-c': componentC
  }
});
除了 new Vue 實體外，也可以在其他組件中使用！

    Vue.component('component-d', {
      components: {
        'component-c': componentC
      },
      template: `
        <component-c></component-c>
      `
    });
## <code>prop</code>屬性
props 屬性的命名及各種不同類型的設定方式。  
https://ithelp.ithome.com.tw/articles/10208500
## 屬性驗證 
props 最簡單的宣告方式就是使用陣列宣告：  

    props: ['name', 'age', 'loveCoding', 'habits', 'education']
但其實也可以用物件定義：  

    props: {
      name: String,
      age: Number,
      loveCoding: Boolean,
      habits: Array,
      education: Object
    },
甚至可以用陣列定義複數種型別：

    ...
    props: {
      ...
      age: [Number, String],
      ...
    },
    ...
### 自訂型別
Vue.js 提供使用者可以用客製建構子檢查型別:

    function Education(university, highSchool) {
        this.university = university;
        this.highSchool = highSchool;
    }

在 props 上直接設定 Education 即可:

    props: {
      education: Education
    },
可以用<code>instanceof</code>來確認兩個型別是否相等。
### 以物件設定物件屬性
下面這個例子，<code>props</code>的值是物件，而各屬性的值也是物件：

    props: {
      age: {
        type: Number,
        required: true,
        default: 0,
        validator: function (value) {
          return value >= 0
        }
      }
    }
當 required 設為 true 的時候，父組件如果沒有傳入屬性值就會跳錯誤訊息。如下例的<code>age</code>:

    props: {
      age: {
        required: true,
      },
    },
如果父組件設定的屬性沒有<code>age</code>，就會報錯:

    <component-all :name="name"
                   :love-coding="loveCoding"
                   :habits="habits"
                   :education="education"></component-all>
                   props: {
這樣可以設定<code>age</code>屬性的預設值:

    age: {
        default: 18,
      },
    },
除了直接設值外，<code>default</code>還可以使用函數設置:

    props: {
      age: {
        default: function() {
            return 18;
        },
      },
    },
## 客製事件
### 事件命名
客製的事件不會轉換名字，他會直接用原本的字串對應，因此如下面的子組件模板:

    <button @click="$emit('plusCount', count++)">+</button>
子組件中觸發<code>plusCount</code>事件，並把 count 加一的結果往上拋，現在看父組件中的設定:

    <counter @plus-count="count=$event"></counter>

父組件在模板上設定的事件名稱是<code>plus-count</code>這樣的kabab-case字串，但因為事件的名稱對應不存在任何的轉換，因此<code>plusCount</code>不會對應到<code>plus-count</code>，所以子組件拋出的<code>plusCount</code>不會觸發<code>plus-count</code>事件。因此最好的方式就是都使用 kabab-case 名稱。如下例：

    <!-- 子組件模板 -->
    <button @click="$emit('plus-count', count++)">+</button>

    <!-- 父組件模板 -->
    <counter @plus-count="count=$event"></counter>
上例才是正確的，所以在事件命名時： 
<code>$emit</code>的事件名稱使用kabab-case。
父組件上綁定的事件名稱使用kabab-case。
### 綁定原生事件
要綁定原生事件可以用<code>native</code>修飾符，這樣Vue.js會知道這個事件是原生的事件而直接綁定到組件的根元素上，如下例的輸入組件:

    Vue.component('base-input', {
      template: `
        <input>
      `
    });
這個組件很簡單，只有一個 <input> 標籤，接著看一下父組件。
template:

    <base-input @focus.native="onFocus"></base-input>
script:

    var vm = new Vue({
      el: '#app',
      methods: {
        onFocus(){
          console.log('focus');
        }
      }
    });
如此一來<code>focus</code>在輸入框時，<code>Console</code>就會輸出<code>focus</code>
### <code>.sync</code> 修飾符
有時屬性也跟 model 一樣會需要做雙向綁定，這時可以用客製事件達成。
一般來說在 Vue.js 中官方建議使用 update:[屬性名] 的事件叫用父組件更新屬性，如下例所示:

    Vue.component('base-button', {
      props: ['title'],
      template: `
        <button @click="click">{{title}}</button>
      `,
      methods: {
        click() {
          const newTitle = this.title.split("").reverse().join("");
          this.$emit('update:title', newTitle);
        }
      }
    });
父組件如下:

    <base-button :title="title" @update:title="title=$event"></base-button>

    var vm = new Vue({
      el: '#app',
      data: {
        title: 'I Love Vue.js'
      },
    });

這樣我們就可在子組件中更新 title 的字串，點擊按鈕會將名稱反序。  
Vue.js 為了讓開發者撰寫的代碼較為單純而提供了<code>.sync</code>修飾符，它是一個屬性雙向綁定的語法糖，因此如下的屬性設定:

    <base-button :title.sync="title"></base-button>
其實就是剛剛的代碼:

    <base-button :title="title" @update:title="title=$event"></base-button>
<code>.sync</code>修飾符不能使用表達式設定，你只能像是<code>v-model</code>那樣使用屬性名稱做設置。
## 插槽
<code>slot</code>的用法。  
https://ithelp.ithome.com.tw/articles/10209348
## 動態元件
和<code>is</code>的用法有關。<code>keep-alive</code>可以解決元件反覆切換卻要被頻繁銷毀的窘境。  
https://ithelp.ithome.com.tw/articles/10209464
## 引入本地static的JSON
https://medium.com/@negarjf/how-to-access-a-static-json-file-in-vue-cli-3-8943dc343f95  
後來是用它示範的解決了（雖然他好像log出錯誤訊息）  
https://segmentfault.com/q/1010000014220437/a-1020000015768253
後來我是這樣  

    mounted () {
        this.fetchMsg()
    }

    data () {
        return {
            items: []
        }
    }

    methods: {
        fetchMsg () {
            axios.get('https://www.ahchoo.cc/note/remsg.php').then(response => {
                return response.data
            }).then(data => {
                console.log(data)
                this.items = data
            }).catch(error => {
                console.log(error.message)
            })
        }
    }
最後的最後，我是這樣用好的，結果已經過了一整個下午......

    import trainingTimeJSON from "../assets/trainingTime.json"
    //import axios from "axios";
    export default {
      name: "ScheduleSection",
      props: {
        section: Number,
        sectionDescription: String
      },
      data: function () {
        return {
          days: [...Array(7).keys()],
          trainingTime: JSON
        };
      },
      mounted: function() {
      },
      methods: {
      },
      created() {
        this.trainingTime = trainingTimeJSON;
        console.log(this.trainingTime);
        console.log(this.trainingTime["1"]);
      },
    };
