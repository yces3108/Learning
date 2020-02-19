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
## Iterator
.forEach()對iterator做一樣的事  

    const fruits = ['mango', 'papaya', 'pineapple', 'apple'];
    fruits.forEach(fruit => {
      console.log('I want to eat a ' + fruit)
    });
.map()對iterator做一樣的事，再變回iterator  

    const bigNumbers = [100, 200, 300, 400, 500];
    const smallNumbers = bigNumbers.map(num => num / 100);
    console.log(smallNumbers);
.filter()也是對iterator做一樣的事，再變回iterator，不過會用條件式篩選  
    
    const favoriteWords = ['nostalgia', 'hyperbole', 'fervent', 'esoteric', 'serene'];
    const longFavoriteWords = favoriteWords.filter(word => {
      return word.length > 7
    });
.findIndex()是找符合條件的東西的index  
.reduce()很有趣，是對每個東西做滾動式的累加  

    const newNumbers = [1, 3, 5, 7];
    const newSum = newNumbers.reduce(function (accumulator,  currentValue){
      console.log('The value of accumulator: ', accumulator);
      console.log('The value of currentValue: ', currentValue);  
      return accumulator + currentValue;
    }, 10);
    console.log(newSum)
.some()是確認至少有一個符合條件的東西  
.every()是確認全都符合條件  
