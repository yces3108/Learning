# Introduction to JavaScript
## Introduction to JavaScript
js是跟console在對話
    
    console.log(25)  
    // 這樣可以打註解  
    console.log(25)  // 這樣也可以  
    /*  
    如果要多行的話  
    這樣就行了!  
    */  
    字串屬性('word'.length()), 字串方法('  word     '.trim())  
## Variables
    var favoriteFood = 'pizza';
    var numOfSlices = 8;
    console.log(favoriteFood);
    console.log(numOfSlices);
var是舊的用法，要宣告變數用新的let和const

    let myName = 'Bruce';
    let myCity = 'Taipei';
    console.log(`My name is ${myName}. My favorite city is ${myCity}.`)  
## Scope
block是一個用大括號{}包成的區塊

    const city = 'New York City';
    function logCitySkyline() {
      let skyscraper = 'Empire State Building';
      return 'The stars over the ' + skyscraper + ' in ' + city;
    }
    console.log(logCitySkyline())
要注意變數的scope，也就是全域(global)、區域(block)
## Arrays
功能: .length(), .push(), .pop(), .join(), .slice(),  
.splice(), .shift()移除第一個, .unshift()從排頭添加, .indexOf(), and .concat()
## Higher-order functions
    const addTwo = num => num + 2;
    const checkConsistentOutput = (func, val) => {
      let firstTry = func(val);
      let secondTry = func(val);
      if (firstTry === secondTry) {
        return firstTry;
      } else {
        return 'This function returned inconsistent results';
      } 
    }
    checkConsistentOutput(addTwo, 100);
