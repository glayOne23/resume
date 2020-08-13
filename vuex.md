## JS
- https://medium.com/better-programming/vuejs-introduction-to-vuex-82bf35be5130
- https://ngide.net/kenalan-dengan-vuex-state-management-di-vue-js/
- https://medium.com/coderupa/panduan-komplit-asynchronous-programming-pada-javascript-part-1-fca22279c056
## Axios
1. npm install axios --save
2. in main.js : set base URL
    ```js
    ---
    import axios from 'axios'
    ---

    axios.defaults.baseURL = 'http://localhost:8000/api/'

    new Vue({
    router,
    store,
    render: h => h(App)
    }).$mount('#app')

    ```
2. in login.vue
    ```html
    <form method="post" @submit.prevent="submit">
        <input class="input is-success" type="text" placeholder="Username" name="name" v-model="form.name">
        <button type="submit" class="button is-success">Login</button>
    </form>

    <script>    

    import axios from "axios";

    export default {

    name: 'Login',

    data () {
        return {
        form : {
            name : '',
            password : '',
        }
        }
    },
    
    methods :{
        async submit() {
        let response = await axios.post('login/', this.form);

        console.log(response.data);
        
        }
    },

    }
    </script>
    ```
## States, Mutations, and Actions in Vuex
1. in store/index.js
    ```js
    import Vue from 'vue'
    import Vuex from 'vuex'
    import auth from './auth'

    Vue.use(Vuex)

    export default new Vuex.Store({
    state: {
    },
    mutations: {
    },
    actions: {
    },
    modules: {
        auth
    }
    })
    ```
2. create store/auth.js :
    ```js
    import axios from "axios";


    export default ({

        //enable namespace
        namespaced : true,

        state: {

            token : null,
            user : null,

        },

        mutations: {

            // set state token
            SET_TOKEN (state, token) {
                state.token = token
            },

            // set state token
            SET_USER (state, user) {
                state.user = user
            }
            
        },

        actions: {

            // first argument is used to dispatch another action or commit mutation
            async login( {dispatch}, credentials) {
                let response = await axios.post('login/', credentials);
                
                // dispatch attempt action
                dispatch('attempt', response.data.access_token)
            },

            // commit SET_TOKEN mutation
            async attempt ( {commit}, token) {
                
                commit('SET_TOKEN', token)

                // commit SET_USER mutation if login succeed
                try {
                    let response = await axios.get('me/', {
                        headers : {
                            'Authorization' : 'Bearer' + token
                        }
                    }) 
                    commit('SET_USER', response.data)
                } catch (e) {
                    // in case token expired
                    commit('SET_TOKEN', null)
                    commit('SET_USER', null)
                }
            }
            

        },


    })

    ```
3. in Login.vue :
    ```js
    <script>

    import {mapActions} from 'vuex'

    export default {

    name: 'Login',

    data () {
        return {
        form : {
            name : '',
            password : '',
        }
        }
    },
    
    methods :{

        ...mapActions({
        login : 'auth/login'
        }),

        async submit() {
        this.login(this.form);
        }
    },

    }
    </script>
    ```

## Showing We are Autheticated
- tambahan getters pada auth.js yang nanti bisa di map dan digunakan di template

## Catch Error
1. in store/auth.js in action :
    ```js
    async login( {dispatch}, credentials) {
        let response = await axios.post('login/', credentials);
        
        // return dispatch
        return dispatch('attempt', response.data)
    },
    ```
2. create helper/Error.js
    ```js
    export default class Error {

        constructor() {
            this.salah = {}
        }
        
        get(field) {
            if (this.salah[field]) {
                return this.salah[field][0];
            }
        }
        
        record(salah) {
            this.salah = salah;
        }
        
        clear(field) {
            delete this.salah[field];
        }
        
        has(field) {
            if (field in this.salah) {
            return true
            }
        }
        
        anyError() {
            return Object.keys(this.salah).length > 0
        }
    }
    ```
3. in Login.vue :
    ```html
    <template>
    <div class="admin-login has-text-centered">  
    <div class="continer">
        <div class="box">
        <article class="media">    
            <div class="media-content">
            <!-- this -->
            <form method="post" @submit.prevent="submit" @keydown="errors.clear($event.target.name)">
                <div class="content">

                <div class="field">
                    <label class="label">Username</label>
                    <div class="control">
                    <!-- this -->
                    <input class="input is-success" type="text" placeholder="Username" name="name" v-model="form.name">
                    </div>
                    <!-- this -->
                    <span class="help is-danger" v-if="errors.has('name')" v-text="errors.get('name')" ></span>
                </div>

                <div class="field">
                    <label class="label">Password</label>
                    <div class="control">
                    <!-- this -->
                    <input class="input is-success" type="password" placeholder="Password" name="password" v-model="form.password">
                    </div>       
                    <!-- this -->   
                    <span class="help is-danger" v-if="errors.has('password')" v-text="errors.get('password')" ></span>
                </div>
                <hr>

                <div class="buttons is-centered">          
                    <!-- this -->
                    <button type="submit" class="button is-success" :disabled="errors.anyError()">Login</button>
                </div>
                
                </div>
            </form>

            </div>
        </article>
        </div>    
    </div>

    </div>
    </template>


    <script>

    import {mapActions} from 'vuex'
    // this  
    import Error from '@/helper/Error.js';


    export default {

    name: 'Login',

    data () {
        return {
        form : {
            name : '',
            password : '',
        },

        // this  
        errors : new Error(),
        }
    },
    
    methods :{

        ...mapActions({
        login : 'auth/login'
        }),

        async submit() {
        this.login(this.form)
        .then(() => {
            this.$router.replace({
            name : 'Admin'
            })
        })
        .catch((error) => {
            // this  
            this.errors.record(error.response.data)
        })
        }
    },

    }
    </script>
    ```

## Vuex Subscribers and Axios Headers
- Jika habis terautentikasi, token yang dikirim akan selalu ada di header dan akan terpakai setiap kali request
1. di src/main : tambahkan :
    ```js
    require('@/store/subscriber')
    ```
2. buat file subscriber pada store : isi dengan :
    ```js
    import store from '@/store'
    import axios from 'axios'

    // store.subscribe((mutation) : ambil semua mutation yang telah terjadi di store
    store.subscribe((mutation) => {
        // console.log(mutation);
        switch (mutation.type){
            // jika ada mutation yang bernama SET_TOKEN pada file store/auth.js
            case 'auth/SET_TOKEN':
                // console.log(mutation.payload);
                // jika ada mutation.payload, buat setiap header ada Authorzation dengan token sebagai berikut
                if (mutation.payload){
                    axios.defaults.headers.common['Authorization'] = 'Bearer ' + mutation.payload
                    // console.log(axios.defaults.headers.common)
                } else {
                    axios.defaults.headers.common['Authorization'] = null
                }

                break;
        }
    })
    ```

## Local Storage and Reautheticating
- Yang sebelumnya hanya akan menaruh token pada setiap header, tapi jika direfresh page, token akan hilang, makanya butuh menyimpan token pada local storage
- Local Storage hanya bisa menyimpan string, object tidak bisa
1. di store/subscriber :
    ```js
    import store from '@/store'
    import axios from 'axios'

    store.subscribe((mutation) => {
        switch (mutation.type){
            case 'auth/SET_TOKEN':
                if (mutation.payload){            
                    axios.defaults.headers.common['Authorization'] = 'Bearer ' + mutation.payload                
                    localStorage.setItem('token', mutation.payload)
                } else {
                    axios.defaults.headers.common['Authorization'] = null
                    localStorage.removeItem('token')
                }

                break;
        }
    })
    ```
2. di src/main :
    ```js
    store.dispatch('auth/attempt', localStorage.getItem('token'))
    ```
3. di store/auth :
    ```js
    async attempt ( {commit, state}, token) {
        if (token) {                
            commit('SET_TOKEN', token)
        }         

        // jika token pada state tidak ada, code akan berhenti disini
        if (!state.token) {                
            return
        }           

        try {
            let response = await axios.get('me/') 
            commit('SET_USER', response.data)
        } catch (e) {
            // in case token expired
            commit('SET_TOKEN', null)
            commit('SET_USER', null)
        }
    }
    ```
## Prevent Flickering
1. in main.js
    ```js
    store.dispatch('auth/attempt', localStorage.getItem('token'))
    .then(() => {
        
        new Vue({
        router,
        store,
        render: h => h(App)
        }).$mount('#app')

    })
    ```
## Simple Middleware
1. in router/index.js :
    ```js
    {
        path: '/login',
        name: 'Login',
        component: () => import('../views/Login.vue'),
        beforeEnter: (to, from, next) => {     
        if (store.getters['auth/authenticated']) {
            return next({
            name : 'Admin'
            })
        }  
        next()
        }
    },
    {
        path: '/admin',
        name: 'Admin',
        component: () => import('../views/Admin.vue'),
        beforeEnter: (to, from, next) => {
        if (!store.getters['auth/authenticated']) {
            return next({
            name : 'Login'
            })
        }
        next()
        }
    }
    ```
## Logout
1. di store/auth actions:
    ```js
    logout({commit}) {
        return axios.post('logout')
            .then(()=> {
                commit('SET_TOKEN', null)
                commit('SET_USER', null)
            })
    }
    ```
2. di AdminNavbar.vue :
    ```html
    <a class="button is-danger" @click.prevent="logout">
        <strong>Logout</strong>
    </a>          

    <script>

    import {mapActions} from 'vuex'

    export default {

    name: 'AdminNavbar',

    methods : {
        ...mapActions({
        logoutAction : 'auth/logout'
        }),

        logout(){
        this.logoutAction().then(() => {
            this.$router.replace({
            name : 'Login'
            })
        })
        }
    }
    }  
    </script>
    ```
## Deploy to Heroku
1. First
```shell
$ npm run build
$ npm install express server-static --save
$ npm install --save connect-history-api-fallback
```
2. create server.js
```js
var express = require('express')
var history = require('connect-history-api-fallback')
var path = require('path')
var serveStatic = require('serve-static')


var app = express()

// Use a fallback for non-root routes (required for Vue router)
//   NOTE: History fallback must be "used" before the static serving middleware!
app.use(history({
    // OPTIONAL: Includes more verbose logging
    verbose: true
}))

// Serve static assets from the build files (images, etc)
app.use(serveStatic(path.join(__dirname, '/dist')))

var port = process.env.PORT || 5000

app.listen(port, () => {
  console.log('Server started at http://localhost:5000')
})
```
3. in package.json :
```js
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint",
    // add this
    'start': "node server.js"
  },
```
4. in .gitignore delete : /dist
5. in heroku :









