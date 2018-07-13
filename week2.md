# Week 2 Web Backend(旧生)

## JavaScript Selector
---

* `document.querySelector`
    - Use to select HTML element or tag based on ID or the HTML class name
    - Select element based on ID
        -   `document.querySelector('#hello')`
    - Select element based on class
        -   `document.querySelector('.hello')`
* `document.getElementById`
    -   Select element only using id (without #)
    -   `document.getElementById('el-id')`
    -   return single HTML String
* `document.getElementByClassName`
    - Select element only using className
        -   `document.getElementByClassName('className')`
        -  return in a collection of HTML syntax

## Use JavaScript update html / text

### Example 1 Using JavaScript set element inner html

Update `<div id="content">` child with `<h1>Hello World</h1>`

``` html
<html>
    <head></head>
    <body>
        <div id="content"></div>
    </body>
    <script>
        document.getElementById('content').innerHTML = "<h1>Hello World</h1>"
    </script>
</html>

```

<center>-- OR --</center>

``` html
<html>
    <head></head>
    <body>
        <div id="content"></div>
    </body>
    <script>
        document.querySelector('#content').innerHTML = "<h1>Hello World</h1>"
    </script>
</html>

```

### Extra:  JavaScript Variable and DataType

#### DataType

| Datatype | Example |
|:----:|:----:|
| Integer | `1`|
|Double | `0.2`|
|String | `1` or `hello` |
| Array| [1,2,3,4] or ["hello" , "world"]|
|Object| {name: "David"}|
| Boolean | true, false|
#### Declare variable and assign value

```js
var a = "hello";
var b = 1;
var names = ["David", "Wilson", "Clinton"]

```

#### Reassign variable value

``` js
// continue on previous code

a = "another Hello";

```

### Example 2 Using JavaScript append child html into element

Append `<h1>hello world</h1>` into #content

``` html
<html>
    <head></head>
    <body>
        <div id="content"></div>
    </body>
    <script>
        var targetContent = document.getElementById('content');
        var element = document.createElement("H1");
        var elementText = document.createTextNode("hello world");
        var text = element.appendChild(elementText);
        targetContent.appendChild(text);
        
    </script>
</html>

```

### Example 3 Using Javascript append child text into element

``` html
<html>
    <head></head>
    <body>
        <div id="content"></div>
    </body>
    <script>
        var targetContent = document.getElementById('content');
        targetContent.append("1");
        
    </script>
</html>

```

# Array, Map and Object

Array - use to store a collection of string, item.

Map - Unique key and value

Object - An object can contains many attributes, usually use to describe an item such as vehicle, person

## Array

### Example 1: Create Array

```js
var arr = ["John", "Wilson", "Brian"];
```

### Example 2 : Append Array item

```js
var arr = ["John", "Wilson", "Brian"];
arr.push("Dennis");
```

### Example 3 : Remove array item start from last

```js
var arr = ["John", "Wilson", "Brian"];
arr.pop()
```

### Example 4 : Remove array item start from 0 index until 1

``` js
var arr = ["John", "Wilson", "Brian"];
arr.slice(0,1);
```

## Map

### Example 1 : Create Map

``` js

var map = new Map();

```
### Example 2 : Assign key and value into map
```js
// continue last code

map.set("key" , "value")

```

### Example 3 : Read value based on map key

```js
//continue last code
map.get("age")
```

### Example 4 : Delete item based on key
```js

//continue last code

map.delete("age")

```

## Object

Object usually use to describe a thing like person, item which contains it's own attribute.


### Example 1 : Create an Object
```js

var myself =  {
    name: "John Doe",
    age: 20,
    hobbies: ["coding", "UI Design" ]

}

```

### Example 2 : Read Object attribute
```js
//continue last code

console.log(myself.name)
// Output : "John Doe"

```

### Example 3 : Set Object Item
```js
// continue last code
myself.name = "John Smith"

```
# For Loop and Foreach Loop

## For Loop
For loop basiclly which tells the machine to execute code repeatly until it fulfill the condition

### Example 1: Count to 4
```js

var maxLoop = 4;

for(var counter = 0; counter <= 4;counter ++){
    console.log(counter);
}
```

## Foreach Loop
Foreach loop mostly use to display each item from an array normally foreach which known as a method of array.

```js

var planet = ["Mars", "Venus" , "Earth" ];

planet.forEach(planet => {
    console.log(planet)
})
```

# Select Input Value

It is easy to select html input value to perform arithmetic calculation or store those text into array or value by using `document.querySelector` .

### Example 1 : Select an input value from console
```html
<html>
    <head></head>
    <body>
        <div id="content">
            <input id="msg" type="text"/>
        </div>
    </body>
    <script>
        
    </script>
</html>

```
Console

Parse input value to string
``` js
var message = document.querySelector('#msg').value.toString();
console.log(message)
```

### Example 3 : Doing some arithmetic

Be caution, only integer, float or double can perform arithmetic in javascript, if the value is in string. For instances `"1"` you can use the parsInt() method to convert the string to integer.

```html
<html>
    <head></head>
    <body>
        <div id="content">
            <input id="num1" type="text" placeholder="number 1"/>
            <input id="num2" type="text" placeholder="number 2"/>
        </div>

        </style>
    </body>
    <script>
        
    </script>
</html>

```
Console

Parse input value to string
``` js
var num1 = document.querySelector('#num1').value;
var num2 = document.querySelector('#num2').value;

num1 = parseInt(num1)
num2 = parseInt(num2)

console.log(num1 + num2)
```

# Handling Event and method

Sometimes, we need an event to trigger a methods for instances, when a button is clicked an alert box will prompt out, as we know this is an onclick event that the funtions need to listen on.


### Example 1: Add an onclick event listener into a div, and trigger a alert window

```html
<html>
    <head></head>
    <body>
        
        <div class="cube" id="cube1">
        <span> Click Me </span>
        </div>
        <style>
        .cube {
            background-color:red;
            height:150px;
            width:150px;
        }
        </style>
    </body>
    <script>
        document.getElementById("cube1").addEventListener('click', () => {
            window.alert("Hello");
        })
    </script>
</html>
```

### Example 2: Handle an input event
```html
<html>
    <head></head>
    <body>
        <input type="text" id="input-01" />
        <span id="res"></span>
        <style>
        .cube {
            background-color:red;
            height:150px;
            width:150px;
        }
        </style>
    </body>
    <script>
        document.getElementById("input-01").addEventListener('input', () => {
            document.getElementById('res').text = document.getElementById("input-01").value
        })
    </script>
</html>
```

### Example 3: Handle a change event

Change event mostly trigger while an input value change, it could be select, radiobutton, text input or checkbox


```html
<html>
    <head></head>
    <body>
        <input type="text" id="input-01" />
        <span id="res"></span>
        <style>
        .cube {
            background-color:red;
            height:150px;
            width:150px;
        }
        </style>
    </body>
    <script>
        document.getElementById("input-01").addEventListener('change', () => {
            document.getElementById('res').text = "Something Changed"
        })
    </script>
</html>
```

# Function

In programming world, we need to reduce the code redundancy.For instance, we can create a function that use to calculate an average with a sets of data passedin.

## Declare a function

A function which accept array of number for average
```js
function average(number){
    var numberLength = number.length;
    var total = 0;
    number.forEach(num => {
        total += num;
    })
    return parseFloat(total / numberLength);
}

```

## Call Function When Event Triggered
``` html
<html>
    <head></head>
    <body>
        <input type="text" id="input-01" />
        <button type="button" id="addNumber">Add Number To Array</button>
        <span id="res"></span>
        <style>
        .cube {
            background-color:red;
            height:150px;
            width:150px;
        }
        </style>
    </body>
    <script>
        var numbers = [];
        document.getElementById('addNumber').addEventListener('click', () => {
            var inputNum = document.getElementById('input-01').value;
            numbers.push(parseInt(inputNum));
            alert(average(numbers))
        });
        function average(number){
            var numberLength = number.length;
            var total = 0;
            number.forEach(num => {
            total += num;
            })
            return parseFloat(total / numberLength);
        }

        
    </script>
</html>
```


# Condition

Condition which known as the important part in the software application. A condition usually use to check certain requirement based on user input and return back to user based on different scenario. For instance, if today weather forecast has a 80% chance of rain then I need to bring an umbrella. So that is a condition occured in our real life.

```js

var chanceOfRain = 80;
var weatherForecast = "rain";
if(weatherForecast === "rain" && chanceOfRain >= 80) {
    bringUmbrella();
}

```

AND Gate


| True | False| Result |
|:--:|:--:|:--:|
| 1   | 0 | 0|
|0  | 1 | 0
|1 | 1 | 1
| 0 | 0| 0


OR Gate

| True | False| Result |
|:--:|:--:|:--:|
| 1   | 0 | 1|
|0  | 1 | 1
|1 | 1 | 1
| 0 | 0| 0


### Example 1:  Writing an If Statement

```js
var condition = true;
if( condition ) {
    // if the condition is true 
    console.log("true")
} else {
    // if the condition is false
    console.log("false")
}
```

### Example 2 : Compare Value

```js
var mark = 80;
if(mark >= 60){
    console.log("You passed the exam")
} else {
    console.log("You failed the exam")
}

```

### Example 3 : String comparison and elseif usage
```js
var grade = "A";
if(grade === "A"){
    console.log("Approx. Mark 80 - 100")
} else if (grade =="B"){
    console.log("Approx. Mark 60 - 79")
} else if(grade == "C") {
    console.log("Approx. Mark 50 - 59")

} else if (grade == "D") {
    console.log("Approx. Mark 30 - 49")

}
```
### Example 4 : OR Gate in If Statement
```js
var booleanA = false;
var booleanB = 50 > 2;

if(booleanA || booleanB) {
   console.log("condition met")
}
//Output condition met

```

### Example 5 : AND GATE in If Statement

```js
var chanceOfRain = 80;
var weatherForecast = "rain";
if(weatherForecast === "rain" && chanceOfRain >= 80) {
    bringUmbrella();
}
```

## Flip the condition with NOT

```js
var weatherForecast = "rain";
if(weatherForecast !== "rain") {
    console.log("Outdoor activities")
} else {
    console.log("Indoor Activities")
}

```


# Welcome Vue

VueJS is a MVVM software architecture progressive JavaScript framework, which mean you can integrate your vue be a part of your web app.


View - ViewModel - Model

## Create Vue Instances

<p>Integrate VueJS into webpage is quite simple, either you can pull in the vue.js library by referencing on a CDN (Content Delivery Network) link in your html head or download the entire vueJS library and include it in your html for offline testing purpose.</p>

### Step 1 : Include Vue CDN link in your HTML head section
```html
<html>
    <head>
       <script src="https://unpkg.com/vue"></script> 
    </head>
    <body></body>

    <script></script>
</html>

```

### Step 2 : Create A div element to allow Vue attach on it

```html
<html>
    <head>
       <script src="https://unpkg.com/vue"></script> 
    </head>
    <body>
        <div id="app">

        </div>
    </body>

    <script>
        
    </script>
</html>

```

### Step 3 : Create a vue instances mount it in the element

```html
<html>
    <head>
       <script src="https://unpkg.com/vue"></script> 
    </head>
    <body>
        <div id="app">
            {{msg}}
            <!-- Print out vue data msg attribute value  -->
        </div>
    </body>

    <script>
        const vue = new Vue({
            el: "#app",
            data: {
                msg: "Hello World"
            },
            methods: {

            },
            mounted() {
                // mounted will be called when Vue is mounted on the element
            }
        })
    </script>
</html>

```

## Inside Vue Constructor Parameters

Obviously, Vue is coded based on top the OOP (Object Oriented Programming). So when a `new vue()` is called out, an Vue object is created with it's own attribute such as el which mean element need to mount on and data which is a object contains a sets of data, mounted which is a method will be called out when Vue is mounted on the element.

```js
const vue = new Vue({
            el: "#app",
            data: {
                msg: "Hello World"
            },
            methods: {

            },
            mounted() {
                // mounted will be called when Vue is mounted on the element
            }
        })
```

## The VueJS Dev Tools

VueJS dev tools is a web browser plugin allow developer to interact with vue instances data attribute or debugging code using console.

VueJS Dev Tools will be available on Google Chrome Webstore

## The directive

**v-model**

v-model usually use to model a data between vue data object and the view, when the value on view (HTML) get updated, at the same time the vue data object will get updated simultaneously.

The below code indicate when the vue msg data model attribute get updated the span tag text will get updated on the same time.

```html
<html>
    <head>
       <script src="https://unpkg.com/vue"></script> 
    </head>
    <body>
        <div id="app">
            {{msg}}
            <!-- Print out vue data msg attribute value  -->
            <input type="text" v-model="msg">
            <span>{{msg}}</span>
        </div>
    </body>

    <script>
        const vue = new Vue({
            el: "#app",
            data: {
                msg: "Hello World"
            },
            methods: {

            },
            mounted() {
                // mounted will be called when Vue is mounted on the element
            }
        })
    </script>
</html>

```
**v-for**

v-for directive normally use to render a list it could be an array of item or objects.

Below code will be rendered out as a list based on the data model names object.
```html
<html>
    <head>
       <script src="https://unpkg.com/vue"></script> 
    </head>
    <body>
        <div id="app">
            {{msg}}
            <!-- Print out vue data msg attribute value  -->
            <input type="text" v-model="msg">
            <ul>
                <li v-for="name in names">{{name}}</li>
            </ul>
        </div>
    </body>

    <script>
        const vue = new Vue({
            el: "#app",
            data: {
                names: [
                    "Bob", "Jonathan", "Tim"
                ]
            },
            methods: {
                
            },
            mounted() {
                // mounted will be called when Vue is mounted on the element
            }
        })
    </script>
</html>

```
