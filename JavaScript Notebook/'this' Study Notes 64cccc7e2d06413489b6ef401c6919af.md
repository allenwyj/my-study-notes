# 'this' Study Notes

Created: Oct 22, 2020
Created by: Allen Wu

# 'This' Variable

## **Normal Situation - Not using arrow function**

- In a method, `this` refers to the **owner object.**
- In a function, `this` refers to the **global object**.
- In a function, in strict mode, `this` is `undefined`.
- In an event, `this` refers to the **element** that received the event.
- Alone, `this` refers to the **global object**

---

### **Regular function call**

- the 'this' keyword points at the global object, (the window object, in the browser);

    ```jsx
    function functionA () {
    	console.log(this);
    }
    functionA();
    // output: window object
    ```

- even though an regular function call is inside another function or method, it will point to the global object = window object.

    ```jsx
    function functionB() {
    	functionA();
    }
    functionB();
    // output: window object
    ```

### **Method call - calling by an object**

- The 'this' keyword points to the object that is calling the method.
- The 'this' keyword is not assigned a value until a function where **(in which object)** it is defined is actually called.

---

## Using **Arrow Function**

### Lexical this Variable.

- An arrow function does not have its own `this`.
- The `this` value of the enclosing lexical scope is used;
- **They simply get the `this` keyword of the function they are written in. Share the lexical `this` keyword from its surroundings. (有点在上一层找this的感觉）**
- Arrow functions follow the normal variable loopup rules.
- 总结：
    - 箭头函数没有自己的this值
    - 箭头函数中使用的this都是来自函数的作用域链
    - 取值遵循普通变量一样的规则，在函数作用域链中一层一层往上找

### **Rules to follow to avoid this problem**

- 对于需要使用object.method()方式调用的函数，使用普通函数定义，不要使用箭头函数。对象方法中所使用的this值有确定的含义，指的就是object本身。

    ```jsx
    // ES5
    var box5 = {
      color: "green",
      position: 1,
      clickMe: function () {
        // 'this' will be assigned value when the method gets called.
        **var self = this;**
        document
    			.querySelector(".green")
    			.addEventListener("click", function () {
    	      // 'this' keyword in this function points to the Window object.
    	      // so, we use 'self' instead
    	      var str =
    	        "This is box number " + self.position;
    	      alert(str);
    	    });
      }
    };
    box5.clickMe();

    // ES6
    // by using arrow function, 'this' keyword in the call-back function is
    // now pointing to the box6 object instead of the window object
    var box6 = {
      color: "green",
      position: 1,
      clickMe: function () {
        document
    			.querySelector(".green")
    			.addEventListener("click", function () {
    	      // 'this' keyword in this function points to the Window object.
    	      // so, we use 'self' instead
    	      var str =
    	        "This is box number " + self.position;
    	      alert(str);
    	    });
      }
    };
    box6.clickMe();
    ```

- 其他情况下，全部使用箭头函数。

    ```jsx
    function Person(name) {
      this.name = name;
    }

    // ES5
    Person.prototype.myFriends5 = function (friends) {
      console.log(this); // points to Person object.
      var arr = friends.map(function (el) {
        console.log(this); // points to window object.
        return this.name + " is friends with " + el;
      });
      console.log(arr);
    };

    // trick to avoid 'this' problem, or we can use var self = this.
    Person.prototype.myFriends5 = function (friends) {
      console.log(this); // points to Person object.
      var arr = friends.map(
        function (el) {
          console.log(this); // points to Person object.
          return this.name + " is friends with " + el;
        }.bind(this)
      );
      console.log(arr);
    };

    var friends = ["a", "b"];
    new Person("Allen").myFriends5(friends);

    // ES6
    Person.prototype.myFriends6 = function (friends) {
      console.log(this); // points to person object.
      var arr = friends.map((el) => {
        console.log(this); // points to person object.
        return `${this.name} is friends with ${el}`; // this.name: Allen
      });
      console.log(arr);
    };
    ```