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

---
## Part 12 - Components Communication I - Communication from child to parent
- One component notify another component
- Example before it: alert message when on blur in one component :
1. index.html
    ```html
    <body>

        <div id="root" class="container">        
            
            <coupon></coupon>

        </div>

        <script src="vue.js"></script>

    </body>

    <script src="main.js"></script>
    ```
2. main.js
    ```js
    Vue.component ('coupon', {
        
        data () {
            return {
            message : "" ,
            }
        }, 

        template : 
            `
            <input type="text" @blur="onCouponApplied" v-model="message">
            ` ,
        
        methods : {
            onCouponApplied(){
                alert(this.message);            
            }        
        }

    });

    new Vue ({

        el : '#root',

    });

    ```
- Example : alert message when on blur (communication from child to parent ) :
1. index.html
    ```html
    <body>

        <div id="root" class="container">    

            <!-- @applied is custom event -->
            <!-- onCouponApplied() is method -->
            <!-- $val catch value from $emit in coupon component -->
            <coupon @applied="onCouponApplied($val)" ></coupon>

        </div>

        <script src="vue.js"></script>

    </body>

    <script src="main.js"></script>
    ```
2. main.js
    ```html
    Vue.component ('coupon', {
        
        data () {
            return {
            message : "" ,
            }
        }, 

        <!-- on blur trigger onCouponApplied method -->        
        template : 
            `
            <input type="text" @blur="onCouponApplied" v-model="message">
            ` ,
        
        methods : {
            
            onCouponApplied(){            
                <!-- emit custom event 'applied' in parent -->
                <!-- this.message send a value to custom event 'applied' -->
                this.$emit('applied', this.message);
            }
        }

    });

    new Vue ({

        el : '#root',

        methods :{
            <!-- parent method -->
            onCouponApplied(message){
                alert(message);
            }
        }
    });

    ```
---
## Part 13 - Components Communication II - Communication between Component
- https://medium.com/@jeffochoa/vue-basic-component-communication-using-an-event-dispatcher-be4823f006bf

---
## Part 14 - Named Slot
1. in index.html
    ```html
    <body>

        <div id="root" class="container">        
            
            <modal>
                <!-- named slot -->
                <template slot="header">Header</template>
                Content of Modal
            </modal>

        </div>

        <script src="vue.js"></script>

    </body>
    ```
2. in main.js
    ```html
    Vue.component ('modal', {
        
        props : [],

        data () {
            return {
            message : "" ,
            }
        }, 

        template : 
            `
            <div class="modal is-active">
                <div class="modal-background"></div>
                <div class="modal-card">
                    <header class="modal-card-head">
                    <p class="modal-card-title">
                        <!-- named slot -->
                        <slot name="header"></slot>
                    </p>
                    <button class="delete" aria-label="close"></button>
                    </header>
                    <section class="modal-card-body">
                    <!-- default slot -->
                    <slot>
                        Default content here
                    </slot>
                    </section>
                    <footer class="modal-card-foot">
                    <button class="button is-success">Save changes</button>
                    <button class="button">Cancel</button>
                    </footer>
                </div>
            </div>
            ` ,
        
    });

    new Vue ({

        el : '#root',

        data : {

        },

    });

    ```

---
## 15. Single Use Components and Inline Components
1. in index.html 
    ```html
    <!-- inline template -->
    <progress-view inline-template>
        <div>
            <h1>Your progress through this course is {{completionRate}} %</h1>

            <button class="button is-info" @click="completionRate += 1">Add Completion</button>
        </div>
    </progress-view>
    ```
2. in main.js
    ```js
    Vue.component ('progress-view', {
        
        data () {
            return {
            completionRate : 50,
            }
        }, 
        
    });

    new Vue ({

        el : '#root',

    });

    ```

---
## 16. Webpack and Vue CLI
- vue-loader is a loader for webpack that allows you to author Vue components in a format called Single-File Components (SFCs):
- Webpack is a static module bundler for modern JavaScript applications. When webpack processes your application, it internally builds a dependency graph which maps every module your project needs and generates one or more bundles.
- SFCs : https://vue-loader.vuejs.org/spec.html
- Vue CLI is a full system for rapid Vue.js development : https://cli.vuejs.org/guide/
- Steps :
    ```sh
    # installing vue cli
    $ sudo npm install -g @vue/cli
    $ sudo npm install -g @vue/cli-init
    # create new vue + webpack bundle
    $ vue init webpack-simple my-app
    # open bundle
    $ cd my-app
    $ subl .
    # install package
    $ npm install
    # run vue in hot reload
    $ npm run dev
    ```
- syntax in file .vue is inunderstandable by the browser, therefore we use webpack to make it understandable by using package bundle that already installed inside webpack
- .vue syntax :
    ```html
    <template>
    <!-- template inside here   -->
    </template>

    <script>
    // script inside here
    </script>

    <style>
    /* css style inside here */
    </style>
    ```
- Example : 
1. in App.vue :
    ```js
    // 1) define component
    <template>
    <div id="app">

        <message>
        Hello There
        </message>
        
    </div>
    </template>

    <script>

    // 2) import any components that app requires
    import Message from './components/Message.vue';

    export default {
    name: 'app',
    
    // 3) nested them here
    components : {Message},

    data () {
        return {
        msg: ''
        }
    }
    }
    </script>

    // 4) style css
    <style>
    #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    }

    h1, h2 {
    font-weight: normal;
    }

    ul {
    list-style-type: none;
    padding: 0;
    }

    li {
    display: inline-block;
    margin: 0 10px;
    }

    a {
    color: #42b983;
    }
    </style>
    ```
2. in components/Message.vue
    ```html
    <template>

    <h1> <slot></slot> </h1>

    </template>

    <script>
    export default {
    
    }
    </script>

    <style>

    </style>
    ```

---
## 17. Hot-Module-Replacement (HMR)
- "Hot Reload" is not simply reloading the page when you edit a file. With hot reload enabled, when you edit a *.vue file, all instances of that component will be swapped in without reloading the page.

---
## 18. Axios Introduction (GET)
- the example below using laravel + vue
1. in web.php make simple API :
    ```php
    Route::get('/skills', function () {
        return ['Laravel', 'Vue', 'React', 'Node'];
    });
    ```
2. in welcome.blade.php :
    ```php
    <body>
        <div id="root">

            // syntax 1 
            <ul>
                <li v-for="skill in skills" v-text="skill"></li>
            </ul>

            // syntax 2 : @ to make laravel ignore {{}}
            <ul>
                <li v-for="skill in skills">@{{skill}}</li>
            </ul>

        </div>
        // cdn vue
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>        
    </body>
    // cdn axios
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="/js/main.js"></script>
    ```
3. in /js/main.js :
    ```js
    new Vue({

        el : '#root',

        data : {
            skills : [],
        },

        mounted () {
            axios.get('/skills')
            .then(response => this.skills = response.data);	
        }
        
    });
    ```

---
## 19. Object Oriented Form Part 1
- The idea is to make simple form that contains of 2 input : name and description.
- Feature :
    1. onSubmit input data to database
    2. validation error from db to js
    3. onkeydown delete error message
    4. disable button when there is a error
    5. onSuccess clear input and show alert message
- Steps :
1. set .env
2. in terminal : 
    ```sh
    $ php artisan make:controller ProjectsController -r
    $ php artisan make:model Project -m
    ```
3. in web.php
    ```php
    <?php

    use Illuminate\Support\Facades\Route;
    
    Route::get('/projects/create', 'ProjectsController@create');
    Route::post('/projects', 'ProjectsController@store');
    ```
4. in Project model :
    ```php
    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;

    class Project extends Model
    {
        protected $guarded = [];
    }

    ```
5. in ProjectController :
    ```php
    <?php

    namespace App\Http\Controllers;

    use Illuminate\Http\Request;
    use App\Project;

    class ProjectsController extends Controller
    {

        public function create()
        {        
            return view('projects.create');
        }

        
        public function store(Request $request)
        {        
            $this->validate(request(), [
                'name' => 'required',
                'description' => 'required'
            ]);

            Project::forceCreate([
                'name' => request('name'),
                'description' => request('description')
            ]);

            return ['message' => "Project Created"];
        }
    }

    ```
6. in view/projects/layout.blade.php :
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Document</title>	
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.8.2/css/bulma.min.css">
        <style type="text/css">
            body {
                padding-top: 40px;			
            }
        </style>

    </head>
    <body>
        <div class="container" id="root">
            @yield("main")
        </div>	
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>        
    </body>
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script src="/js/main.js"></script>
    </html>
    ```
7. in view/projects/create.blade.php :
    ```html
    @extends("projects.layout")

    @section("main")


    {{-- ada hal baru @submit.prevent mencegah submit untuk melakukan behaviour normal, makanya disini nggak ada @csrf --}}
    {{-- errors is property in data that contains Errors class we have created in main.js --}}
    {{-- .clear(), .get(), .has() is artificial methods from class Errors that we created --}}
    <form method="POST" action="/projects" @submit.prevent="onSubmit" @keydown="errors.clear($event.target.name)">
        <div class="field">
        <div class="control">
            {{-- v-model --}}
            <input class="input is-success" type="text" name="name" placeholder="Name" v-model="name">	    
            <span class="help is-danger" v-if="errors.has('name')" v-text="errors.get('name')" ></span>
        </div>
        </div>
        <div class="field">
        <div class="control">
            {{-- v-model --}}
            <input class="input is-success" type="text" name="description" placeholder="Description" v-model="description">
            <span class="help is-danger" v-if="errors.has('description')" v-text="errors.get('description')" ></span>
        </div>
        </div>
        {{-- if there is an error, submit button is disabled --}}
        <button type="submit" class="button is-success" :disabled="errors.anyError()" >Save</button>
    </form>
    @endsection
    ```
8. in main.js
    ```js
    class Errors {

        // create local property
        constructor() {
            this.salah = {}
        }

        // if there is an error with specified field, get message
        get(field) {
            if (this.salah[field]) {
                return this.salah[field][0];
            }
        }

        // record error if available from backend to local property
        record(salah) {
            this.salah = salah;
        }

        // on keydown clear message
        clear(field) {
            delete this.salah[field];
        }

        // if there is an error with specified field, show html tag
        has(field) {
            return this.salah.hasOwnProperty(field);
        }

        // if there is an error, disable the button
        anyError() {
            return Object.keys(this.salah).length > 0
        }
    }

    new Vue({

        el : '#root',
        
        data : {
            // related to v-model in create.blade.php
            name : '',
            description : '',
            // property to create Errors class that contains error message
            errors : new Errors(),
        },

        methods : {
            onSubmit() {
                // this.$data is all data in new Vue()
                axios.post('/projects', this.$data)
                // if submit success, trigger onSuccess() method
                .then(this.onSuccess)
                // this.errors is property in data that contains Errors class
                .catch(error => this.errors.record(error.response.data.errors));
            },

            onSuccess(response) {
                // data.message is from backend
                alert(response.data.message);

                // clear all input after successfully posted data to db
                this.name = '';
                this.description ='';
            }
        }
        
    });
    ```

---
## 20. Object Oriented Form Part 2
1. data dimasukkan dalam class Form
2. errors dimasukkan dalam class Form
3. submit dimasukkan dalam class Form
4. reset dimasukkan dalam class Form






