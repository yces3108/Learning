## 字串處理
#### .equals()
<code>.equals()</code>用來判斷兩個物件的內涵是否一致。  
<code>==</code>是當兩個參考指向同一物件時，才會回傳<code>true</code>，  
而<code>.equals()</code>則是先確認兩物件的類別是否一致，才比較其內容值。  
#### String.ToCharArray
<code>String.ToCharArray</code>是將字串變成字元陣列。
例：

    Char[] c = s.toCharArray()
## 常用資料結構
### Priority Queue
#### .add(): 添加物件
#### .poll(): 回傳首位物件
#### .peek(): 偷看首位物件
#### .size(): 回傳大小
#### 若要由小到大排序的話
    PriorityQueue<Integer> queue = new PriorityQueue<>(new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return o2 - o1;
        }
    });

