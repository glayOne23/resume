# 3-4. React CDN and First Component
1. download react.js and react-dom.js 
2. download babel.js
3. in js script that use react add `<script type="text/babel">`
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>

        <script src="reactv16-13.js"></script>
        <script src="react-domv16-13.js"></script>
        <script src="babel.js"></script>
    </head>


    <body>

        <div id="app"></div>

    </body>


    <script type="text/babel">

        class App extends React.Component {
            render() {
                return (
                    <div>
                        <h1>Hei, you</h1>
                    </div>
                )
            }
        }

        ReactDOM.render(<App />, document.getElementById('app'));

    </script>


    </html>
    ```
