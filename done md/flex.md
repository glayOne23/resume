# Flex in CSS
---
## Parent Element (Container)
### flex container
- any tag that set the display property to flex it become flex-container
- any tag inside flex-container can be manipute as flex
- kaedah 1 :
  > A Flexible Layout must have a parent element with the display property set to flex.
- kaedah 2 :
  > Direct child elements(s) of the flexible container automatically becomes flexible items.
- syntax :
``` css
.nama_terserah {
    display: flex;
    }
```
- example :
``` html
<html>
    <head>
    <style>
    .flex-container {
      display: flex;
      background-color: DodgerBlue;
    }

    .flex-container > div {
      background-color: #f1f1f1;
      margin: 10px;
      padding: 20px;
      font-size: 30px;
    }
    </style>
    </head>
    <body>

    <div class="flex-container">
      <div>1</div>
      <div>2</div>
      <div>3</div>
    </div>

    </body>
    </html>
```
- hasil :

  ![](imgflex/flex.png?raw=true)

---
## The flex container properties
  - flex container `display:flex` has 6 properties :
    - flex-direction
    - flex-wrap
    - flex-flow
    - justify-content
    - align-items
    - align-content
### A ) flex-direction property
- The `flex-direction` property defines in which direction the container wants to stack the flex items
  1. `flex-direction: colomn` stacks the flex items vertically (from top to bottom):
      - example :
        ``` html
          <html>
            <head>
              <style>
                .flex-container {
                  display: flex;
                  flex-direction: column;
                  background-color: DodgerBlue;
                }

                .flex-container > div {
                  background-color: #f1f1f1;
                  width: 100px;
                  margin: 10px;
                  text-align: center;
                  line-height: 75px;
                  font-size: 30px;
                }
              </style>
            </head>
            <body>
              <div class="flex-container">
                <div>1</div>
                <div>2</div>
                <div>3</div>  
              </div>
            </body>
          </html>
        ```
      - hasil :

      ![](imgflex/flex2.png?raw=true)
    </br></br>
  2. `flex-direction: column-reverse` stacks the flex items vertically (but from bottom to top)
      - syntax :
        ``` css
        .flex-container {
          display: flex;
          flex-direction: column-reverse;
        }
        ```
      - hasil :

        ![](imgflex/flex3.png?raw=true)
    </br></br>
  3. `flex-direction: row` stacks the flex items horizontally (from left to right)
     - syntax :
        ``` css
        .flex-container {
          display: flex;
          flex-direction: row;
        }
        ```
      - hasil :

        ![](imgflex/flex4.png?raw=true)
    </br></br>
  4. `flex-direction: row-reverse`stacks the flex items horizontally (but from right to left)
      - syntax :
        ``` css
        .flex-container {
          display: flex;
          flex-direction: row-reverse;
        }
        ```
      - hasil :

        ![](imgflex/flex5.png?raw=true)
    </br></br>
### B) flex-wrap property
- The `flex-wrap` property specifies whether the flex items should wrap or not.
  1. `flex-wrap: wrap` specifies that the flex items will wrap if necessary
      - syntax :
        ``` css
          .flex-container {
            display: flex;
            flex-wrap: wrap;
          }
        ```
      - hasil :    

        ![](imgflex/flex6.png?raw=true)   
    </br></br>
  2. `flex-wrap: wrap-reverse` specifies that the flexible items will wrap if necessary, in reverse order
      - syntax :
        ``` css
        .flex-container {
          display: flex;
          flex-wrap: wrap-reverse;
        }
        ```
      - hasil :      

         ![](imgflex/flex7.png?raw=true)     
    </br></br>
  3. `felx-wrap: unwrap` specifies that the flex items will not wrap (this is default)
      - syntax :
      ``` css
        .flex-container {
          display: flex;
          flex-wrap: nowrap;
        }
      ```
      - hasil :

        ![](imgflex/flex8.png?raw=true)     
    </br></br>
### C) flex-flow property
  - The flex-flow property is a shorthand property for setting both the flex-direction and flex-wrap properties.
  - syntax :
    ``` css
    .flex-container {
      display: flex;
      flex-flow: row wrap;
    }
    ```    
    </br>    
### D) justify-content Property    
- The `justify-content` property is used to align the flex items
  1. `center` :  aligns the flex items at the center of the container
      ``` css
        .flex-container {
          display: flex;
          justify-content: center;
        }
      ```
      ![](imgflex/flex9.png?raw=true)     
  2. `flex-start` : aligns the flex items at the beginning of the container (this is default)
      ``` css
        .flex-container {
          display: flex;
          justify-content: flex-start;
        }
      ```  
  3. `flex-end` : aligns the flex items at the end of the container
      ``` css
        .flex-container {
          display: flex;
          justify-content: flex-end;
        }
      ```
      ![](imgflex/flex10.png?raw=true)
  4. `space around` : displays the flex items with space before, between, and after the lines
      ``` css
        .flex-container {
          display: flex;
          justify-content: space-around;
        }
      ```
      ![](imgflex/flex11.png?raw=true)
  5. `space-between` : displays the flex items with space between the lines
      ``` css
        .flex-container {
          display: flex;
          justify-content: space-between;
        }
      ```
      ![](imgflex/flex12.png?raw=true)    
    </br>    
### E) align-items Property
  - is used to align the flex items vertically
    1. `center` :  aligns the flex items in the middle of the container
        ``` css
          .flex-container {
            display: flex;
            height: 200px;
            align-items: center;
          }
        ```
        ![](imgflex/flex13.png?raw=true)
        In these examples we use a 200 pixels high container, to better demonstrate the align-items property.
    2. `flex-start` :  aligns the flex items at the top of the container
        ``` css
          .flex-container {
            display: flex;
            height: 200px;
            align-items: flex-start;
          }
        ```
        ![](imgflex/flex14.png?raw=true)          
    3. `flex-end` :  aligns the flex items at the bottom of the container
        ``` css
          .flex-container {
            display: flex;
            height: 200px;
            align-items: flex-end;
          }
        ```
        ![](imgflex/flex15.png?raw=true)
    4. `stretch` :   stretches the flex items to fill the container (this is default)
        ``` css
          .flex-container {
            display: flex;
            height: 200px;
            align-items: stretch;
          }
        ```
        ![](imgflex/flex16.png?raw=true)              
    5. `baseline` :   aligns the flex items such as their baselines aligns
        ``` css
          .flex-container {
            display: flex;
            height: 200px;
            align-items: baseline;
          }
        ```
        ![](imgflex/flex17.png?raw=true)              
        Note: the example uses different font-size to demonstrate that the items gets aligned by the text baseline
    </br>
### F) align-content Property
  - is used to align the flex lines
    1. `space-between` displays the flex lines with equal space between them
      ``` css
        .flex-container {
          display: flex;
          height: 600px;
          flex-wrap: wrap;
          align-content: space-between;
        }
      ```
      ![](imgflex/flex18.png?raw=true)
      In these examples we use a 600 pixels high container, with the flex-wrap property set to wrap, to better demonstrate the align-content property.
    2. `space-around` displays the flex lines  with space before, between, and after them
      ``` css
        .flex-container {
          display: flex;
          height: 600px;
          flex-wrap: wrap;
          align-content: space-around;
        }
      ```
      ![](imgflex/flex19.png?raw=true)
    2. `stretch` stretches the flex lines to take up the remaining space (this is default)
      ``` css
        .flex-container {
          display: flex;
          height: 600px;
          flex-wrap: wrap;
          align-content: stretch;
      ```   
      ![](imgflex/flex20.png?raw=true)
    3. `center` displays display the flex lines in the middle of the container
      ``` css
        .flex-container {
          display: flex;
          height: 600px;
          flex-wrap: wrap;
          align-content: center;
      ```   
      ![](imgflex/flex21.png?raw=true)
    3. `flex-start`  displays the flex lines at the start of the container
      ``` css
        .flex-container {
          display: flex;
          height: 600px;
          flex-wrap: wrap;
          align-content: flex-start;
      ```   
      ![](imgflex/flex22.png?raw=true)
    3. `flex-end`  displays the flex lines at the end of the container
      ``` css
        .flex-container {
          display: flex;
          height: 600px;
          flex-wrap: wrap;
          align-content: flex-end;
      ```   
      ![](imgflex/flex23.png?raw=true)
---
## Perfect Centering
- SOLUTION: Set both the justify-content and align-items properties to center, and the flex item will be perfectly centered
- syntax :
  ``` css
    .flex-container {
      display: flex;
      height: 300px;
      justify-content: center;
      align-items: center;
    }
  ```
  ![](imgflex/flex24.png?raw=true)
---
## Flexible Item
- The direct child elements of a flex container automatically becomes flexible (flex) items.
---
## Flex item properties
- the flex item property are :
  1. order
  2. flex-grow
  3. flex-shrink
  4. flex-basis
  5. flex
  6. align-self

### A) order
- specifies the order of the flex items.
- The order value must be a number, default value is 0.
- example :
  ``` html
  <div class="flex-container">
    <div style="order: 3">1</div>
    <div style="order: 2">2</div>
    <div style="order: 4">3</div> 
    <div style="order: 1">4</div>
  </div>
  ```  
  ![](imgflex/flex25.png?raw=true)
</br>
### B) flex-grow Property
  - specifies how much a flex item will grow relative to the rest of the flex items.
  - The value must be a number, default value is 0.
  - example :
    ``` html
    <div class="flex-container">
      <div style="flex-grow: 1">1</div>
      <div style="flex-grow: 1">2</div>
      <div style="flex-grow: 8">3</div> 
    </div>
    ```
    ![](imgflex/flex26.png?raw=true)
</br>
### C) flex-shrink Property
  - specifies how much a flex item will shrink relative to the rest of the flex items
  - The value must be a number, default value is 1.
  - example :
    ``` html
    <div class="flex-container">
      <div>1</div>
      <div>2</div>
      <div style="flex-shrink: 0">3</div>
      <div>4</div>
      <div>5</div>
    </div>
    ```
    ![](imgflex/flex27.png?raw=true)
</br>
### D) flex-basis Property
  - specifies the initial length of a flex item.
  - example : 
    ``` html
      <div class="flex-container">
        <div>1</div>
        <div>2</div>
        <div style="flex-basis: 200px">3</div>
        <div>4</div>
      </div>
      ```
      ![](imgflex/flex28.png?raw=true)
</br>
### E) Flex property
  - is a shorthand property for the flex-grow, flex-shrink, and flex-basis properties.
  - example :
    ``` html 
      <div class="flex-container">
        <div>1</div>
        <div>2</div>
        <div style="flex: 0 0 200px">3</div>
        <div>4</div>
      </div>
    ```
    ![](imgflex/flex29.png?raw=true)  
    Make the third flex item not growable (0), not shrinkable (0), and with an initial length of 200 pixels
</br>
### F) align-self Property
  - specifies the alignment for the selected item inside the flexible container.
  - overrides the default alignment set by the container's `align-items` property.
  - example :
    ``` html
      <div class="flex-container">
        <div>1</div>
        <div style="align-self: center">2</div>
        <div style="align-self: flex-end">3</div>
        <div>4</div>
      </div>
    ```
    ![](imgflex/flex30.png?raw=true)  
    Align the second flex item at the center of the container, and the third flex item at the bottom of the container:    
---
---
## Example of Resposive Image Grid
``` html
  <!DOCTYPE html>
  <html>
  <style>
  * {
    box-sizing: border-box;
  }

  body {
    margin: 0;
    font-family: Arial;
  }

  .header {
    text-align: center;
    padding: 32px;
  }

  .row {
    display: flex;
    flex-wrap: wrap;
    padding: 0 4px;
  }

  /* Create four equal columns that sits next to each other */
  .column {
    flex: 25%;
    max-width: 25%;
    padding: 0 4px;
  }

  .column img {
    margin-top: 8px;
    vertical-align: middle;
  }

  /* Responsive layout - makes a two column-layout instead of four columns */
  @media (max-width: 800px) {
    .column {
      flex: 50%;
      max-width: 50%;
    }
  }

  /* Responsive layout - makes the two columns stack on top of each other instead of next to each other */
  @media (max-width: 600px) {
    .column {
      flex: 100%;
      max-width: 100%;
    }
  }
  </style>
  <body>

  <!-- Header -->
  <div class="header">
    <h1>Responsive Image Grid</h1>
    <p>Resize the browser window to see the responsive effect.</p>
  </div>

  <!-- Photo Grid -->
  <div class="row"> 
    <div class="column">
      <img src="/w3images/wedding.jpg" style="width:100%">
      <img src="/w3images/rocks.jpg" style="width:100%">
      <img src="/w3images/falls2.jpg" style="width:100%">
      <img src="/w3images/paris.jpg" style="width:100%">
      <img src="/w3images/nature.jpg" style="width:100%">
      <img src="/w3images/mist.jpg" style="width:100%">
      <img src="/w3images/paris.jpg" style="width:100%">
    </div>
    
    <div class="column">
      <img src="/w3images/underwater.jpg" style="width:100%">
      <img src="/w3images/ocean.jpg" style="width:100%">
      <img src="/w3images/wedding.jpg" style="width:100%">
      <img src="/w3images/mountainskies.jpg" style="width:100%">
      <img src="/w3images/rocks.jpg" style="width:100%">
      <img src="/w3images/underwater.jpg" style="width:100%">
    </div> 
    
    <div class="column">
      <img src="/w3images/wedding.jpg" style="width:100%">
      <img src="/w3images/rocks.jpg" style="width:100%">
      <img src="/w3images/falls2.jpg" style="width:100%">
      <img src="/w3images/paris.jpg" style="width:100%">
      <img src="/w3images/nature.jpg" style="width:100%">
      <img src="/w3images/mist.jpg" style="width:100%">
      <img src="/w3images/paris.jpg" style="width:100%">
    </div>
    
    <div class="column">
      <img src="/w3images/underwater.jpg" style="width:100%">
      <img src="/w3images/ocean.jpg" style="width:100%">
      <img src="/w3images/wedding.jpg" style="width:100%">
      <img src="/w3images/mountainskies.jpg" style="width:100%">
      <img src="/w3images/rocks.jpg" style="width:100%">
      <img src="/w3images/underwater.jpg" style="width:100%">
    </div>
  </div>

  </body>
  </html>

```  
![](imgflex/flex31.png?raw=true)  










