---
title: Vue
created: '2020-12-13T03:44:14.835Z'
modified: '2020-12-17T06:55:30.234Z'
---

# Vue

## Starting with a Vue-3 app with event handlers
### Vue cdn
```html
    <script src="https://unpkg.com/vue@next"></script>
```

```html 
    <div id="events">
        <div class="box" @click="evHandler($event)" @mousemove="mouseHandler($event)" >
            {{x}} &#10060 {{y}}
        </div>
    </div>
```
the event object is automatically passed if no argument is porvided. However if any argument is porvided, 
            event object has to be passed explicitely wiht $event.


```javascript
const events = Vue.createApp({
    data() {
        return {
            x: 0,
            y:0
        }
    },
    methods: {
        evHandler(ev) {
            console.dir(ev.type);
        },
        mouseHandler(ev) {
            this.x = ev.offsetX;
            this.y = ev.offsetY;
        }
    }
})
events.mount('#events');
```

## Vue attributes
| Attribute | Shortcut | Detail                                       |
| --------- | -------- | -------------------------------------------- |
| v-if      |          | Renders an element conditionally             |
| v-show    |          | controls css display                         |
| v-on      | @        | sets event-listener like @click, @mouseenter |
| v-bind    | :        | binds value of attribute to a varible        |
| v-for     |          | iterating over an array                      |


## Binding the class attribute
class attribute can be bound with v-bind. An object is provided which sets the classes based on an expression which reduces to a binary value.

```html
<div v-bind:class={selected:x.isSelected}></div>
```
### Detailed example
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue app</title>
    <link rel="stylesheet" href="style.css">

    <script src="https://unpkg.com/vue@next"></script>
    <script src="app.js" defer></script>

</head>

<body>
    <div id="app">
        <div class="box">
            <div class="book" v-for="x in books" @click="evHandler($event, x)" v-bind:class="{selected:x.selected, test:x.selected}">
                <p>{{x.title}}</p>
            </div>
     
        </div>
    </div>
</body>

</html>
```

```
const app = Vue.createApp({
    data() {
        return {
            books: [
                {
                    title: 'book-1',
                    author:'author-1'
                },
                {
                    title: 'book-2',
                    author: 'author-2'
                },
                {
                    title: 'book-3',
                    author: 'author-3'
                }
          ]
        }
    },
    methods: {
        evHandler(ev,x) {
            console.log(x);
            x.selected = !x.selected;
        }
    }
})
app.mount('#app');
```
```
* {
	box-sizing: border-box;
	font: inherit;
}

html {
	font-family: Verdana, Geneva, Tahoma, sans-serif;
	font-size: 16px;
}

body {
	font-size: 1rem;
}

.box {
	width: 400px;
	height: 300px;

	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;

	/* background:yellow; */
	border: 1px solid lightgrey;
	border-radius: 5px;
}

.book{

	margin:10px;
	align-self:stretch;
	padding:0 10px;
	border-radius: 5px;
	border:1px solid lightgrey;
	color:#444;
}

.book.selected{
	background:red;
	color:white;
}
```


## Vue computed properties
computed properties should be used when calculations are involved on element body or for props which need not to update frequntly.
Coputed props are updated only when their dependencies change. Computed props are computed with a getter funciton, which is defined inside `computed` object.

```
Vue.createApp({
  data(){
    return {
      x:5
    }
  },
  methods:{},
  computed{
    computedProp(){
      return this.x+1;
    }
  }
})
```
```
<div id="computed">
  <span>{{ computedProp }}</span>
</div>
```

Notice how `comptedProp` in involed as a poperty and not as a method.
Somthing similar can be done by defining the `computedProp()` mehtods and invoking it as `computedProp()`.
Computed props are essentially does the same thing except it also caches the values based on their reactive dependencies.
A computed prop will only re-evaluate when some of its reactive dependencies have changed.
>
This also means that following computed prop will never update, because `Date.now()` is not a reactive dependency:
```
computed: {
  now() {
    return Date.now()
  }
}
```

In comparision a method will always run the funciton whenever a re-render happens.
Caching is important for avoiding expansive computation for every render.

Computed props by default is getter only but setters can also be defined. Check the doc.

[Computed props](https://v3.vuejs.org/guide/computed.html#computed-properties)



## Vue event modifiers / filters

```html
<form v-on:submit.prevent="submitForm"></form>
<button @click.right="handler">Right click</button>
<input @keyup.enter></input>
```
To stop propagation of click event use `@click.stop`. No need to assign it to any callback. In the following situation there is an event listener attached to list item which will remove the list item on click. But we do not want to remove the list item when the click is on the input item.

Also notice how index can used with `v-for`.

```html
<li v-for="(x,i) in items" @click="removeItem(i)">
  <input @click.stop v-model="x.inp"></input>
</li>
```

## Watchers  
Watcher is set by repeating a data or computed prop name as a method. This watcher method is executed whtenever the connected data or computed prop changes. Watcher does not return anything but can be updated to update other props or data which depend on this. It is something very similar to ng-change.

```javascript
data(){
  return {
    name:'',
    rank:200
  }
}
watch:{
  name(value, oldValue){
    // it automatically gets updated value and prev value.
    if(name==''){
      this.fullName='';
    }
    else{
      this.fullName = this.name + " something";
    }
  }
}
```
For each property a watcher method is required, therefore, watchers are not that great for calculating dynamic values.
The main logic of watchers is to watch for a propery value and take action whten a certain threshold is hit. For example resetting a counter when  it hits 50. For this kind of tasks, a computed prop is not that effecient.

```javascript
watcher:{
  counter(value){
    if(value>50){
      this.counter=0;
    }
  }
}
```

## Dynamic inline styling with Vue.

With Vue, not only classes could be set dynamically but also inline styles can be set. The syntax is similar to classes, an object is passed which sets CSS properties based on an expression. Here, it can reduce to a valid CSS value, but for classes it was a binary value. CSS property names can be camel cased here, which will be automatically corrected by Vue to dash based names. Or, if dashed names has to be used, CSS property names can be placed inside single quote.

```html
<div class="" :style="{borderColor: isSelected?'red':'grey'}">
<div class="" :style="{'border-color': isSelected?'red':'grey'}">
```
Computed properties can be used for computing the classes instead of inline calculations, 
```html
<mydiv class="", :class="myClasses">
```
```javascript
const app = Vue.createApp({
  data(){},
  computed{
     myClasses(){
       return { selected:this.selected, highlighted:this.highlighted }
     }
  }
})
```
## v-for
```html
<li v-vor="x in items">{{x.name}}</li>
<li v-for="(x,index) in items">{{x}}, {{index}}</li>
```

Elements rendered with v-for should be provided with an optional `key` attribute to assign unique identity to that DOM element.
It is important because for poerformance optiomization reasons, whenever a DOM element is removed, Vue replaces it contents with the following DOM element instead of completely removing it and re-rendering the DOM. 

This behaviour can cause strange bugs like unwanted removal of input box contents etc. It is to be noted here that index is not a good fit for `key` attribute because the index is dynamic and after removal of an item, the same index is assigned to the following item. `key` therefore should be derived from some kind of id or unique values from the data. `key` can be bound to some value with `v-bind` or `:` attribute.

```html
<li v-for="(x,i) in items" v-bind:key="x.name">x.name</li>
```




## Vue cli
```
npm install -g @vue/cli
vue create vue-app

cd vue-app
npm run serve
```














































































