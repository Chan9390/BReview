### Chapter 4 : JavaScript - An Object-Based Language

##### Creating Objects
- You can create JS object using the following syntax:
`var MyVariable = new ObjectName(optional parameters);`

##### Some objects

- var a = new Date("1 Jan 2017");
- var b = new String("Hello");`

#####  Some notable array operations:
- length : `myarray.length` displays the length of the array, i.e. no of elements
- sort() : `myarray.sort()` sorts the array in ascending order

##### JavaScript Native Objects

**String Objects :** `var a = new String("");`

Object | Description
---|---
length 	| `a.length` - displays the lenght of the string
charAt() | `a.charAt()` - displays the character at a specified position in the string
charCodeAt()	| `a.charCodeAt()` - similar to charAt() but returns the decimal character code (ASCII) for the character
fromCharCode()| `String.fromCharCode(65, 66, 67)` - converts the ASCII numbers to string
indexOf()	| `a.indexOf("a")` - used for searching the occurence of one string inside another i.e. substring and returns the index (-1 if not found)
lastIndexOf()	| `a.lastIndexOf("a")` - starts searching from the end
substring()	| `a.substring(0,4)` - prints first 4 letters of the string `a`
substr()	| `a.substr(1)` - denotes how many characters should be cut out from the beginning; second parameter is optional; the example displays the string without the first character
toLowerCase()	| `a.toLowerCase()` - converts the string `a` to lowercase
toUpperCase()	| `a.toUpperCase()` - converts the string `a` to uppercase

**Math Object :**

Object | Description
---|---
abs()		| `Math.abs(num)` - returns absolute value; i.e. the positive value of `num`
ceil()	| `Math.ceil(num)` - returns the nearest integer greater than `num`
floor()	| `Math.floor(num)` - returns the nearest integer lesser than `num`
round()	| `Math.round(num)` - returns the rounded value of `num`
random()	| `Math.random()` - returns a random value between 0 and 1
pow()		| `Math.pow(10, 2)` - returns 10^2

**Number Object :** `var a = new Number(123)`

Object | Description
---|---
toFixed()	| `a.toFixed(num)` - returns a value with `num` decimal places after the decimal point; note that the last value is rounded off if it had some more decimal places

**Array Object :** `var a = new Array(1,2,3)`

Object | Description
---|---
length	| `a.length` - returns the length of the array
concat()	| `a.concat(another_array)` - concates two arrays into the former; in the example `a` gets appended with another array `another_array`
slice()	| `a.slice(num1, num2)` - returns a part of the array starting from index `num1` till index `num2`; but the last index i.e. `a[num2]` is not included is the array
join()	| `a.join('<br>')` - converts array into a sinlge string with the given parameter / string in between the array elements; in the example the array elements are joined with `<br` in between
sort()	| `a.sort()` - sorts the array in ascending order and replaces the original array with the sorted one; `sort` is case sensitive - 'Paul' will come before 'paul'
reverse()	| `ar.reverse()` - reverses the order of the elements; replaces the original array with sorted one

**Date Objects :** `var a = new Date()`

- Creating Date objects:
	- `var a = new Date()` : the date and time will be set to the current date and time on the PC
	- `var b = new Date(949278000000)` : here milliseconds is passed; in the example the date is 31st Jan 2000 00:20:00 GMT
	- `var c = new Date("31 January 2010")` : here the date is passed as a string; the date could also be written as `31 Jan 2010`, `Jan 31 2010`, `01-31-2010`; but the last one `01-31-2010` is not recognized by some browsers as valid date input
	- `var d = new Date(2010, 0, 31, 15, 35, 20, 20)` : this sets the time to 31 Jan 2010, 15:35:20 and 20 milliseconds
	- Setting Date values
		- `a.setDate()` : date is passed as parameter
		- `a.setMonth()` : month is passed as parameter; note hat the first month is 0 and the last month is 11
		- `a.setFullYear()` : 4 digit year is passed as parameter
		- **NOTE: Theres no way for web-based JS to change current date and time of users system**
- Calculations and Dates (some examples)
	- Example 1
	  ```JavaScript
	  var a = new Date("1 Jan 2010");
	  myDate.setDate(32);
	  document.write(myDate);
	  ```
	  As Jan has only 31 days, the date will be set to 1st Feb (automatically changes month)
	- Example 2
	  ```JavaScript
	  var nowDate = new Date();
	  var currentDay = nowDate.getDate();
	  nowDate.setDate(currentDay + 28)
	  ```
	  Increments the date by 28 days
	- Example 3
	  ```JavaScript
	  nowDate.setDate(currentDate - 28)
	  ```
	  Decrements the date by 28 days
- Getting time values
			- `getHours()`
			- `getMinutes()`
			- `getSeconds()`
			- `getMilliseconds()`
			- `getTimeString()`
		- Setting time values
			- `setHours()`
			- `setMinutes()`
			- `setSeconds()`
			- `setMilliseconds()`

#### JavaScript Classes

- Class is generally a template for an object
- Class consists of:
	- Constructor
	- Method definition
	- Properties
- You need to construct a constructor, even if no initialization
- Example constructor (for class `CustomerBooking`):
  ```JavaScript
  function CustomerBooking (bookingId, customerName, film, showDate)
  {
    this.customerName = customerName;
    this.bookingId = bookingId;
    this.film = film;
    this.showDate = showDate;
  }
  ```
- `this` refers to object instance of the class
- Please make sure you write the correct spelling for the parameters, as JS will not mention if there is any errors in names
- Syntax of defining a class:
  ```JavaScript
  classname.prototype.methodname = function(method parameter list)
  {
    //method code
  }
  ```
- Creating and using class object instances : syntax
  ```JavaScript
  var something = new classname(parameters if any)
  ```
- For convenience, its better to put the code of class in a separate file and call it when required using the syntax:
  `<script language="JavaScript" src="MyCinemaBookingClasses.js"></script>`
- Example for entering an array of items
  ```JavaScript
  function cinema() {
    this.bookings = new Array();
  }
  cinema.prototype.addBooking = new function (bookingId, customerName, film, showDate) {
    this.bookings[bookingId] = new CustomerBooking(bookingId, customerName, film, showDate);
  }
  ```
- The above example creates an array of objects belonging to `CustomerBooking` class
