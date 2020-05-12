## Part 1 - Basic Data Binding
- automatic data binding : bind the value of html to property in js. Either one of them changes, the other will changes
- service area : place where vue object will work
- every word begin with v- is directive
- v-model : specify model to bind
- {{ var_name }} : get the value of variable in data
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <title>Vue Laracast</title>
    </head>
    <body>

        <!-- service area -->
        <div id="first">
            <input type="text" id="input" v-model="message">
            <p>the value of the input is : {{message}} </p>
        </div>
        <!-- end service area -->

        <script src="vue.js"></script>

    </body>

    <script>

        // cara manual
        // document.querySelector("#input").value = "hai"

        // cara vue
        new Vue({
            //el identify what service area this vue will work
            el: "#first",
            //model of this vue
            data : {
                message : "Hello World"
            }
        })

    </script>


    </html>
    ```
## Part 2 - Setup Vue Devtools
- Steps :
    1. install vue extension
    2. give the extensions access to file url
- He explain single source of truth

## Part 3 - Lists
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Vue Laracast</title>
</head>
<body>

    <div id="first">
        <ul>        
            <!-- cara 1 -->
            <li v-for="name in names">{{name}}</li> 

            <!-- cara 2 -->
            <li v-for="name in names" v-text="name"></li> 
        </ul>


    </div>

    <script src="vue.js"></script>

</body>

<script>

    var app = new Vue({
        el: "#first",
        data : {
            names : ["hammam", "islami", "muhammad"]
        }
    })

</script>

</html>
```
## Part 3.1 - Event Listeners (Manual)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Vue Laracast</title>
</head>
<body>

    <div id="first">
        
        <ul>        
            <li v-for="name in names" v-text="name"></li> 
        </ul>

        <input type="text" name="" id="input">

        <button id="button">Add Name</button>
        

    </div>

    <script src="vue.js"></script>

</body>

<script>

    var app = new Vue({
        
        el: "#first",
        
        data : {
            names : ["hammam", "islami", "muhammad"]
        },

        mounted(){
            document.querySelector('#button').addEventListener('click',() =>{
            let name = document.querySelector('#input');
            app.names.push(name.value);
            name.value = "";
        });

        }

    });

</script>

</html>
```
- Result

    ![](img_vue_laracast/1.png?raw=true)

## Part 4 - Vue Event Listeners
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Vue Laracast</title>
</head>
<body>

    <div id="first">
        
        <ul>        
            <li v-for="name in names" v-text="name"></li> 
        </ul>

        <!-- bind the value to vue data newName -->
        <input type="text" v-model="newName">

        <!-- event listener onclick call addName() -->
        <button v-on:click="addName">Add Name</button>
        <!-- shortkey to v-on: is @ -->
        <button @click="addName">Add Name</button>
        
    </div>

    <script src="vue.js"></script>

</body>

<script>

    var app = new Vue({
        
        el: "#first",
        
        data : {
            newName : "",
            names : ["hammam", "islami", "muhammad"]
        },

        methods: {
            // to push value of input into names
            addName() {
                this.names.push(this.newName);

                this.newName = ''
                
            }
        },
        
    });


</script>

</html>
```

## Part 5 - Atrribute and Class Name Binding
- Bind attribute and class to data in vue
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <title>Vue Laracast</title>
        
        <style>
            .warna {
                color: blue;
            }
        </style>
    </head>
    <body>

        <div id="root">

        <!-- bind attribute to data in vue -->
        <!-- syntax normal -->
        <button v-bind:title="title">Hover over me</button>
        <!-- shorthand -->
        <button :title="title">Hover over me</button>


        <!-- bind class to data in vue -->
        <h3 :class="kecss">Hello Word</h3>

            
        </div>

        <script src="vue.js"></script>

    </body>

    <script>

        var app = new Vue({
            
            el: "#root",
            
            data : {
                title : "The new title",
                //warna will reference to class in css
                kecss : "warna"
            },

            
            
        });


    </script>

    </html>
    ```
- Conditional class using css
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <title>Vue Laracast</title>
        
        <style>
            .warna {
                color: blue;
            }
        </style>
    </head>
    <body>

        <div id="root">

        <!-- warna = class in css; kondisi_warna = data with value true or false -->
        <!-- look at curly bracket, we use {} because {warna: kondisi_warna} is an object in js-->
        <p :class ="{warna: kondisi_warna}">Hello Word</p>
        <button @click="ubahWarna">Ubah Warna Menjadi Normal</button>
        
        </div>

        <script src="vue.js"></script>

    </body>

    <script>

        var app = new Vue({
            
            el: "#root",
            
            data : {            
                kondisi_warna : true,
            },

            methods: {
                ubahWarna (){
                    this.kondisi_warna = false;
                }
            }

            
            
        });

    </script>

    </html>
    ```
---
## Part 6 - Computed Property
1. Example 1 : To reverse word : without using computed property : look so messy :
    ```html
    <body>

        <div id="root">

            <p>{{message}} </p>
            <!-- this one -->
            <p> {{message.split('').reverse().join('')}} </p>
        
        </div>

        <script src="vue.js"></script>

    </body>

    <script>

        var app = new Vue({
            
            el: "#root",
            
            data : {            
                message : "Hello World",
            },

        });


    </script>
    ```
2. Example 2 : To reverse word : using computed property :
    ```html
    <body>

        <div id="root">

            <p>{{message}} </p>
            <!-- computed property -->
            <p> {{reverseMessage}} </p>
        
        </div>

        <script src="vue.js"></script>

    </body>

    <script>

        var app = new Vue({
            
            el: "#root",
            
            data : {            
                message : "Hello World",
            },

            <!-- computed property -->
            computed : {
                reverseMessage (){
                    return this.message.split('').reverse().join('')
                }
            }

        });

    </script>
    ```
3. Example 3 : To list completed taks using computed property :
    ```html
    <body>

        <div id="root">

            <h1>All Task</h1>
            <li v-for="task in tasks"> {{task.description}} </li> 
            <h1>Completed Task</h1>
            <!-- this one -->
            <li v-for="task in completedTaks"> {{task.description}} </li> 
        
        </div>

        <script src="vue.js"></script>

    </body>

    <script>

        var app = new Vue({
            
            el: "#root",
            
            data : {            
                tasks : [
                    {description : "Go to store", completed : true},
                    {description : "Go to Mall", completed : true},
                    {description : "Buy some stuff", completed : false},
                    {description : "Get money", completed : true},
                    {description : "Pray", completed : false},
                ]
            },

            computed : {
                // this one
                completedTaks (){
                    return this.tasks.filter(task => task.completed);
                }
            }

        });

    </script>
    ```
---

## Part 7 - Components Introduction
1. in index.html :
    ```html
    <body>

        <div id="root">

            <!-- this is vue component -->
            <task>Go to store</task>

        </div>

        <script src="vue.js"></script>

    </body>
    <!-- import main.js that include vue component template inside it -->
    <script src="main.js"></script>
    ```
2. in main.js
    ```html
    <!-- vue component template -->
    <!-- Vue.component ('component-name', {}) -->
    Vue.component ('task', {
        
        <!-- template -->
        <!-- using backtick -->
        <!-- look at that slot it's used to get value between tag-->
        template : 
            `<li>
                <slot></slot>
            </li>`

    });

    new Vue ({

        el : '#root',

    })
    ```
---

## Part 8 - Components II - Component Within Component
- in index.html :
    ```html
    <body>

        <div id="root">
            <!-- this one -->
            <task-list></task-list>

        </div>

        <script src="vue.js"></script>

    </body>

    <script src="main.js"></script>
    ```
- in main.js : 
    ```html
    Vue.component ('task-list', {
       <!-- 1) u must wrap the element with div because : [Vue warn]: Error compiling template: Cannot use v-for on stateful component root element because it renders multiple elements. -->
       <!-- 2) u should have explicit keys for every loop because : component lists rendered with v-for should have explicit keys -->
        template : 
            `
            <div>
            <task v-for="task in tasks" :key="task.id">
                {{task.description}}
            </task>
            </div>
            ` ,
        <!-- 3) look at this one : to pass data, instead of using data : {} u use data() { return {} because it is inside component-->
        data() {
            return {
                tasks : [
                    {id : 1, description : "Go to store", completed : true},
                    {id : 2, description : "Go to Mall", completed : true},
                    {id : 3, description : "Buy some stuff", completed : false},
                    {id : 4, description : "Get money", completed : true},
                    {id : 5, description : "Pray", completed : false},
                ]        
            }
        }

    });

    Vue.component ('task', {
        
        template : 
            `<li>
                <slot></slot>
            </li>`

    });

    new Vue ({

        el : '#root',

    })
    ```
---

## Part 9 - Practical Components Exercise I (Props, v-show, and Data) - Message 
1. use bulma for styling
    ```html
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Vue Laracast</title>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.8.2/css/bulma.min.css">
        <script defer src="https://use.fontawesome.com/releases/v5.3.1/js/all.js"></script>
    </head>
        <style>
            body {
                margin-top: 40px;
            }
        </style>
    ```
2. in index.html :
    ```html
    <body>

        <div id="root" class="container">
            <!-- 1) title and body is props not attributes -->
            <message title="Hai it's First" body="Lore ipsum dolar shit amet"></message>    
            <message title="Hai it's Second" body="Lore ipsum dolar shit amet"></message>    
            <message title="Hai it's Third" body="Lore ipsum dolar shit amet"></message>    
        </div>

        <script src="vue.js"></script>

    </body>

    <script src="main.js"></script>
    ```
3. in main.js
    ```html
    Vue.component ('message', {

        <!-- 1) this is props -->
        props : ["title", "body"],

        <!-- 2) this is data related to message component -->
        data () {
            return {
                isVisible : true,
            }
        }, 

        template : 
            `
            <!-- 3) v-show will show the component if the value is true -->
            <article class="message" v-show="isVisible" >
                <div class="message-header">
                    <!-- 1.1) title prop -->
                    <p> {{title}} </p>
                    <!-- 4) change isVisible value to false on click -->
                    <button class="delete" @click="isVisible=false" aria-label="delete"></button>
                </div>
                <div class="message-body">
                    <!-- 1.2) body prop -->
                    {{body}}
                </div>
            </article>
            ` ,
    });

    new Vue ({

        el : '#root',

    })
    ```
---

## Part 10 - Practical Components Exercise II (Communication to Component and Custom Event) - Modal
- How to communicate to component ? eg : to show modal on click button and when x buttton is clicked, the modal disappear, if button is not part of the modal component.
- main.js : 
    ```html
    Vue.component ('modal', {
        
        template : 
            `
            <div class="modal is-active">
                <div class="modal-background"></div>
                <div class="modal-content">
                    <div class="box">
                        <slot></slot> 
                    </div>
                </div>            
                <!-- 1) this is will emit @tutup in index.html on click -->
                <button class="modal-close is-large" aria-label="close" @click="$emit('tutup')"></button>
            </div>
            ` ,
    });

    new Vue ({

        el : '#root',

        <!-- 2) global property -->
        data : {
            showModal : false,
        }
    });

    ```
- index.html : 
    ```html
    <body>

        <div id="root" class="container">

            <!-- 1) v-show="showModal" at first it will return false -->
            <!-- 3) @tutup is custom event -->
            <modal v-show="showModal" @tutup="showModal=false">
                <p>Lore ipsum imet chogito ergo</p>
            </modal>

            <!-- look at this ! button is not part of modal component -->
            <!-- 2) on click show modal -->
            <button class="button" @click="showModal=true" > ShowModal </button>

        </div>

        <script src="vue.js"></script>

    </body>

    <script src="main.js"></script>
    ``` 
- in main.js, we cant do like this : `<button class="modal-close is-large" aria-label="close" @click="showModal = false"></button>` because the scope different, showModal is global property

---
## Part 11 - Practical Components Exercise III () - fsdaf

