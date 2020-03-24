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
        <!-- look at curly bracket -->
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