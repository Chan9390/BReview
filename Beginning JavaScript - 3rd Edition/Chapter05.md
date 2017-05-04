### Chapter 5: Programming the Browser

#### `window` object
- It is a global object, which means that you dont need to use its name to access its properties and methods

#### `history` object
- keeps track of websites you visit; this list is called 'history stack'
- `history.length` : shows how many pages are in the history stack
- `history.forward()` : loads the next page
- `history.back()` : loads the previous page
- `history.go(5)` : goes 5 pages forward
- `history.go(-5)` : goes 5 pages backward

#### `location` object
- contains useful info about current page's location
- `location.href` : returns the URL of the page
- `location.hostname` : server hosting it; generally the domain name
- `location.port` : port number of the server connection
- `location.protocol` : protocol used (http or https)
- `location.assign("https://google.com")` : loads google.com
- `location.replace("https://google.com")` : loads google.com
- `location.href = "https://google.com"` : loads google.com
- `replace` replaces the current URL in the history stack, whereas `href` adds the new website to the stack

#### `navigator` object
- it is the browser object; gives information about the browser
- `navigator.userAgent` : returns the user agent of the browser

#### `screen` object
- information about the display capabilities of client machine
- `screen.height` : vertical range of the screen in pixels
- `screen.width` : horizontal range of the screen in pixels
- `screen.colorDepth` : number of bits used for colors on the client screen

#### `document` object
- `document.write()` : writes the content into the html page

##### `images` array

- Syntax of inserting an image:
  `<img alt="IMAGE" name=MyImg src="myimg.jpeg">`
- In above example, the image (in `img` object) has a name `MyImg`
- **All images are stored in `images[]` array**
- `document.images.length` : returns the number of elements in an array
- Useful example:
  ```JavaScript
  <img name=img1 src="" border=0 width=200 height=150>
  <script language="JavaScript" type="text/javascript">
    var myimages = new Array("image1.jpg", "image2.jpg", "image3.jpg");
    var imageIndex = prompt("Enter a number from 0 to 2",0);
    document.images["img1"].src = myimage[imageIndex];
  </script>
  ```
- *In the above example please note that the `.src` is used to denote the source of the image is being assigned*

##### `links` array

- for each hyperlink tag `<A>` defined in `href`, browser creates an `A` object
- all the hyperlinks are stored in the `links[]` array

#### Connecting code to page events

- Events occur when something in particular happens; ex: user clicks on the page/button
- event handlers are made of the word `on`; example: `onclick`, `onload`
- Easy way of connecting the event to the code is to add it directly to the tag of the object whose event you are capturing
- Example:
  ```html
  <A href="somepage.html" name="linkSomePage" onclick="alert('You clicked?')">
    Click Me
  </A>
  ```
- If you have number of lines to be connected, then write a function and call it in the tag
- Example:
  ```html
  <script language="JavaScript">
  function linksomepage_onclick() {
    alert('You clicked?');
    return true;
  }
  </script>
  <A href="somepage.html" name="linkSomePage" onclick="return linksomepage_onclick()">
    Click Me
  </A>
  ```
- In the above example, if you had returned `false`, the event might have occured, but would not go to `somepage.html`
- **Not all objects and their events make use of `return` value. Its not always that `false` cancels the action, sometimes returning `true` cancels the action.**
- Event handlers for `window` object actually goes into `body`
- Example:
  ```html
  <body language="JavaScript" onload="myOnLoadfunction()" onunload="myOnUnloadfunction()">
  ```

#### Event Handlers as Properties

- You need to first define the function that will be executed when the event occurs
- Example:
  ```JavaScript
  <script type="text/javascript">
    function somefunction() {
      //code
    }
  </script>
  <script language="JavaScript" type="text/javascript">
    window.document.links[0].onclick = somefunction;
  </script>
  ```
- **Note that no parenthesis is added to the function name while assigning it as a property**

#### Browser Version Checking Examples

- Two ways to check for browser details:
  - to check whether a particular object or property is actually available in users browser (as BOM differs from each browser and versions)
  - checking the browser using `navigator` object, particularly `navigator.userAgent` or `navigator.appName`
    - `navigator.appName` returns the model of the browser, such as MS IE or Netscape
    - `navigator.userAgent` returns a string containing information about the browser including its version

##### No Script at all

- Sometimes the users disable javascript in the browser / may use a browser which doesnt support JS
- Use `<noscript>` in such situations
- Example:
  ```html
  <body>
  <noscript>
    This website requires JavaScript
  </noscript>
  </body>
  ```
