### Chapter 2 : Data Types and Variables

- JS is weakly typed language; user dont need to specify the type of the data - automatically assigns
- Floating point numbers will be rounded of to integers
- Strings can be enclosed with both " and '

- you can use hexadecimal using \x and Unicode escape sequence as \u

  Ex: \xA9 is the same as \u00A9

- boolean data types: `true` and `false`

- declare variables using `var <var_name>`
- input the data using `prompt`

Ex:
  ```
  var something;
  something = prompt("Enter something", 50);
  ```
  In the above example 50 is the default value which will be present in the prompt box

---
**My ideas:**

Short XSS script:

```
<script type="text/javascript">
var a = prompt("Input something");
document.write(a);
</script>
```

Payloads:
- `<script>alert(1);</script>`
- `</script><script>alert(1);</script>`
- `<script>document.write(prompt(""))</script>`, then enter the `<script>alert(1);</script>`

---

- To concat two strings, use `+` symbol
Ex: `var smthng = "Chan" + "Paul"`

- To use the input as numbers use `parseInt()` and `parseFloat()`
- if a non-integer value is passed into the input, then the above commands - parseInt() and parseFloat() returns `NaN` which means Not-a-Number
- You can check if the input is number or not using `isNaN`, where if the input is a number then false is returned else true
- You can declare a new array using `new Array()`
  Ex:
  ```
  var smthng = new Array();
  ```
- To initialize the number of elements in the array, just mention that in the brackets
  Ex: `var smthng = new Array(15);`
- You could also initialize the array when you declare, using
  Ex: `var smthng = new Array("paul", 356, "john", 112);`

- You cant declare an array with just one element

- Some interesting snippet:
  ```
  <script type="text/javascript">
  var a = new Array("Hello", 1, 2);
  document.write(isNaN(a[2]));
  document.write("This: " + parseInt(a[1]+a[2]));
  </script>
  ```

- You can also explicitly mention during the input
Ex:
`var a = Number(prompt("Enter something"));`

- Creating multidimentional array, for each element you have to initialize a new array
Ex:
  ```
  var a = new Array(5);
  a[0] = new Array();
  a[1] = new Array();
  a[2] = new Array();
  ```
