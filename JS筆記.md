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
## Object
this指涉calling object本身  
物件的private屬性用底線(object._attribute)，不過這只是大家的慣例  
getter就是在物件內部傳值的  
setter則是變更物件內部的值  

    const robot = {
      _model: '1E78V2',
      _energyLevel: 100,
      _numOfSensors: 15,
      get numOfSensors(){
        if(typeof this._numOfSensors === 'number'){
          return this._numOfSensors;
        } else {
          return 'Sensors are currently down.'
        }
      },
      set numOfSensors(num) {
        if (typeof num === 'number' && num >= 0) {
          this._numOfSensors = num;
        } else {
          console.log('Pass in a number that is greater than or equal to 0');
        }
    }
    };
呼叫setter

    robot.numOfSensors = 100;
呼叫getter

    console.log(robot.numOfSensors);
Factory

    function robotFactory(model, mobile){
      return {
        model,
        mobile,
        beep() {
          console.log('Beep Boop');
        }
      }
    }
    // To check that the property value shorthand technique worked:
    const newRobot = robotFactory('P-501', false)
Destructured Assignment: 直接把物件的值拿出來用

    const robot = {
      model: '1E78V2',
      energyLevel: 100,
      functionality: {
        beep() {
          console.log('Beep Boop');
        },
        fireLaser() {
          console.log('Pew Pew');
        },
      }
    };
    const {functionality} = robot;
    functionality.beep();
## Classes
extends用來繼承父類別class.
super()呼叫父類別的constructor()
Static method可以直接從class叫,不用再生成它的instance
## Broswer compatibility and transpilation
caniuse.com — A website that provides data on web browser compatibility for HTML, CSS, and JavaScript features. You will learn how to use it to look up ES6 feature support.  
Babel — A Javascript library that you can use to convert new, unsupported JavaScript (ES6), into an older version (ES5) that is recognized by most modern browsers.  
https://medium.com/@cheling/javascript-%E7%80%8F%E8%A6%BD%E5%99%A8%E5%85%BC%E5%AE%B9%E6%80%A7%E5%92%8C%E8%BD%89%E6%8F%9B-110f49ef0c4
## Intermediate javascript modules
要export的話就像這樣

    export default Airplane;
    module.exports = Airplane; (Node.js不支援)
要import的話就像這樣(需要對面先export才能對接)  

    const Airplane = require('./2-airplane.js');
export有分一般的(export)或是export default
## Promise
Pending, resolve, and reject  
Promise有點像是機會命運，選了機會會怎樣，選了命運會怎樣  
Pending指的是它的等待狀態  
有做到要求就是resolve，沒做到就是reject  

    const myExecutor = (resolve, reject) => {
        if (inventory.sunglasses > 0) {
            resolve('Sunglasses order processed.');
        } else {
            reject('That item is sold out.');
        }
    }
    const orderSunglasses = () => {
      return new Promise(myExecutor);
    }
    const orderPromise = orderSunglasses();
當你先設定完之後  

    const checkInventory = (order) => {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                let inStock = order.every(item => inventory[item[0]] >= item[1]);
                if (inStock) {
                    resolve(`Thank you. Your order was successful.`);
                } else {
                    reject(`We're sorry. Your order could not be completed because some items are sold out.`);
                }
            }, 1000);
        })
    };
你就可以用.then()來讓它等待callback function  
.catch()其實一樣，只是它只抓失敗的

    const handleSuccess = (parameter) => {
      console.log(parameter);
    }
    const handleFailure = (parameter) => {
      console.log(parameter);  
    }
    checkInventory(order).then(handleSuccess, handleFailure);
一堆promise是可以串在一起的  

    checkInventory(order)
    .then((resolvedValueArray) => {
      // Write the correct return statement here:
     return processPayment(resolvedValueArray);
    })
    .then((resolvedValueArray) => {
      // Write the correct return statement here:
      return shipOrder(resolvedValueArray)
    })
    .then((successMessage) => {
      console.log(successMessage);
    })
    .catch((errorMessage) => {
      console.log(errorMessage);
    });
Promise.all()裡面可以放一個array的promise一並解決
## Async await
async await是ES8裡promise的語法糖

    //第一種打法
    async function myFunc() {
      // Function body here
    };
    //第二種打法
    const myFunc = async () => {
      // Function body here
    };
原本的

    function withConstructor(num){
      return new Promise((resolve, reject) => {
        if (num === 0){
          resolve('zero');
        } else {
          resolve('not zero');
        }
      })
    }    
可以簡化成這樣

    async function withAsync(num) {
      if (num === 0){
          return('zero');
        } else {
          return('not zero');
        }
    }
然後用await可以直接等到resolution過來

    // Native promise version:
    function nativePromiseDinner() {
      brainstormDinner().then((meal) => {
          console.log(`I'm going to make ${meal} for dinner.`);
      })
    }
    // async/await version:
    async function announceDinner() {
      let meal = await brainstormDinner();
      console.log(`I'm going to make ${meal} for dinner.`);
    }
## Request
One of JavaScript’s greatest assets is its non-blocking properties, or that it is an asynchronous language.  
用event loop來應對非同步的呼叫  
Asynchronous JavaScript and XML (AJAX)  
XMLHttpRequest (XHR) API  
JSON: JavaScript Object Notation  
GET樣板:

    const xhr = new XMLHttpRequest(); 
    const url = 'https://api-to-call.com/endpoint';
    xhr.responseType = 'json';

    xhr.onreadystatechange = () => {
      if (xhr.readyState === XMLHttpRequest.DONE) {
        return xhr.response;
      }
    }

    xhr.open('GET', url);
    xhr.send();

POST樣板:

    const xhr = new XMLHttpRequest();
    const url = 'https://api-to-call.com/endpoint';
    const data = JSON.stringify({id: '200'});
    xhr.responseType = 'json';
    xhr.onreadystatechange = () => {
      if (xhr.readyState === XMLHttpRequest.DONE) {
        return xhr.response;
      }
    }
    xhr.open('POST', url);
    xhr.send(data);
要在URL後面加query的話，在最後面加?  
若query有很多參數，用&區隔，然後對每個key-value pair用=對應  

GET, POST如果寫成fetch()會方便很多(ES6, ES7)

    fetch('https://api-to-call.com/endpoint')
      .then(response => {
      if (response.ok) {
        return response.json();
      }
      throw new Error('Request failed!');
    }, networkError => {
      console.log(networkError.message);
    }).then(jsonResponse => {
      return jsonResponse;
    });

而且還可以寫成async await(ES8)

    const getData = async () => {
      try {
        const response = await fetch('https://api-to-call.com/endpoint');
        if (response.ok) {
          const jsonResponse = await response.json();
          return jsonResponse;
        }
        throw new Error('Request failed!');
      } catch (error) {
        console.log(error);
      }
    }
