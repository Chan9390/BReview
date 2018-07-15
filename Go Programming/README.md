## Go Programming - Agus Kurniawan

### 1. Development environment

Go version - `go version`

Hello world program: (`main.go`)
```
package main
import "fmt"
func main() {
  fmt.Println("Hello, go!")
}
```

**Always import packages as strings i.e. within quotes**
**There should always be a main() function**, or else you get an error `go run: cannot run non-main package`

To build and run : `go build` & `go run main.go`

### 2. Go Programming Language

- Dont use `;` at the end
- Declaring variables:
  - `var mydata data_type1`
  - `var n, m data_type1`
  - ```
    var (
      name string
      email string
      age int
    )
    ```
- Assigning Variables
  - `n = 10`
  - `var city string = "London"`
  - `country := "DE"`

- Comments
  - `//`
  - `/* */`

#### Arithmetic operations

```
package main
import "fmt"
func main() {
	var a int = 5
	var b int = 10
	c := a - b
	fmt.Printf("%d - %d = %d \n", a, b, c)
	d := float64(a) / float64(b)
	fmt.Printf("%d / %d = %.2f \n", a, b, d)
}
```
(This doesnt work with `Println`)

#### Mathematical Functions

```
package main
import (
	"fmt"
	"math"
)

func main() {
	a := 2.4
	c := math.Pow(a,2)
	fmt.Printf("%.2f^%d = %.2f \n", a, 2, c)
}
```

- Increment and Decrement
  - `a++` = a + 1
  - `a--` = a - 1

#### Getting input from keyboard

```
package main
import (
	"fmt"
	"math"
)

func main() {
	fmt.Println("Circle Area Calculator")
	fmt.Print("Enter the radius: ")
	var radius float64
	fmt.Scanf("%f", &radius)
	area := math.Pi * math.Pow(radius,2)
	fmt.Printf("Area: %.4f \n", area)
}
```

#### Comparison Operators

- `==`
- `!=`
- `>=`
- `<=`
- `>`
- `<`

#### Logical operators

- `&&` - and
- `||` - or
- `!`  - not

#### Decision

Can use either if..then or switch..case

**if..then**

```
package main
import "fmt"
func main() {
  var (
	a = 5
	b = 8
  )
  if a>b || a-b<a {
    fmt.Println("conditional->a>b || a-b<a")
  }else{
    fmt.Println("..another>")
  }
}
```

**switch..case**

```
package main
import "fmt"
func main() {
  selected := 2
  switch selected {
    case 0:
      fmt.Println("selected = 0")
    case 1:
      fmt.Println("selected = 1")
    case 2:
      fmt.Println("selected = 2")
    default:
      fmt.Println("other..")
  }
}
```

**for loop**

```
package main
import "fmt"
func main() {
  var i int
  for i=0; i<5; i++ {
	fmt.Println(i)
  }
  for j:=5; j<11; j++ {
	fmt.Println(j)
  }
}
```

**while loop**

- Golang doesn't provide while syntax like C langauge. But we can use `for` loop

```
package main
import "fmt"
func main() {
	i := 0
	for i<5 {
		fmt.Println(i)
		i++
	}
}
```

**break and continue**

- break : to stop on the code point
- continue : to skip some scripts

```
package main
import "fmt"
func main() {
	var i int
	for i=0; i<5; i++ {
		if i==3 {
			break
		}
		fmt.Println(i)
	}
	for j:=5; j<11; j++ {
		if j==7 {
			continue
		}
		fmt.Println(j)
	}
}
```

### 3. Arrays, Slices and Maps

- Array objects
  - `var numbers[5] int`
  - `var cities[5] string`
  - `var matrix[3][3] int`

- **Slice** : we can define an array without giving array length. Go uses Slice to do this case.

```
package main
import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("define slices")
	var numbers[] int
	numbers = make([]int, 5)
	matrix := make([][]int,3*3)

	fmt.Println(">>>>insert slice data")
	for i:=0; i<5; i++ {
		numbers[i] = rand.Intn(100) // random number
	}

	fmt.Println(">>>>>> insert slice matrix data")
	for i:=0; i<3; i++ {
		matrix[i] = make([]int,3)
		for j:=0;j<3;j++ {
			matrix[i][j] = rand.Intn(100)
		}
	}
}
```

**Map** : an arry with key-value

```
package main
import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("define map")
	products := make(map[string]int)
	customer := make(map[string]int)

	fmt.Println(">>>>>> insert map data")
	products["product1"] = rand.Intn(100)
	products["product2"] = rand.Intn(100)

	customers["cust1"] = rand.Intn(100)
	customers["cust2"] = rand.Intn(100)

	fmt.Println(produces["product1"])
	fmt.Println(produces["product2"])
	fmt.Println(customers["cust1"])
	fmt.Println(customers["cust1"])
}
```

### 4. Functions

**Creating a function**

```
func function_name return_datatype {
	return foo
}
```

**Create simple function**

```
func main() {
	foo()
}

func foo() {
	fmt.Println("foo() was called")
}
```

**Functions with Parameters**

```
func circle_area(r float64) {
	area := math.Pi * math.Pow(r, 2)
	fmt.Printf("Circle area (r= %.2f) = %.2f \n", r, area)
}
```

```
func calculate(a, b, c float64) {
	result := a*b*c
	fmt.Println("a=%.2f, b=%.2f, c=%.2f \n", a, b, c, result)
}
```

**Function with returning value**

```
func advanced_calculate(a, b, c float64) float64 {
	result := a*b*c
	return result
}
```

**Function with multiple return values**

```
func compute(a, b, c float64, name string) (float64, float64, string) {
	result1 := a*b*c
	result2 := a + b + c
	result3 := result1 + result2
	newName := fmt.Sprintf("%s value = %.2f", name, result3)

	return result1, result2, newName
}
```

**Function with Multiple Parameters and returning value**

```
func add(numbers ...int) int {
	result := 0
	for _, number := range numbers {
		result += number
	}
	return result
}
```

**Closure function** : functions in a function

```
func closure_func(name string) {
	hoo := func(a,b int){
		result := a*b
		fmt.Printf("hoo() = %d \n", result)
	}
	joo := func(a, b int) int {
		return a*b + a
	}

	fmt.Printf("Closure_func(%s) was called\n", name)
	hoo(2,3)
	val := joo(5,8)
	fmt.Printf("val from joo() = %d \n", val)
}
```

**Recursion Function**

```
func fibonacci(n int) int {
	if n==0 {
		return 0
	} else if n==1 {
		return 1
	}
	return (fibonacci(n-1) + fibonacci(n-2))
}
```

### 5. Pointers

```
package main
import "fmt"
func main() {
	var x int
	x = 10
	fmt.Println(x)
	fmt.Println(&x)
}
```

To set memory locations

```
package main
import "fmt"
func main() {
	var x int
	x = 10
	fmt.Println(x)
	fmt.Println(&x)

	var num *int
	val := new(int)

	num = new(int)
	*num = x

	val = &x

	fmt.Println("===pointer num===")
	fmt.Println(*num)
	fmt.Println(num)
	fmt.Println("===pointer val===")
	fmt.Println(*val)
	fmt.Println(val)
}
```

**Singly Linked List**

```
package main
import (
	"fmt"
	"math/rand"
)

type Node struct {
	value int
	next *Node
}

func add(list *Node, data int) *Node {
	if list==nil {
		list := new(Node)
		list.value = data
		list.next = nil
		return list
	} else {
		temp := new(Node)
		temp.value = data
		temp.next = list
		list = temp
		return list
	}
}

func display(list *Node) {
	var temp *Node
	temp = list

	for temp!=nil {
		fmt.Println(temp.value)
		temp = temp.next
	}
}

func main() {
	var head *Node
	head = nil

	fmt.Println("=== insert 5 data===")
	n := 0
	for n < 5 {
		fmt.Printf("data %d\n", n)
		head = add(head, rand.Intn(100))
		n++
	}

	fmt.Println(">>> display ===")
	display(head)
}
```

### 6. Structs and Methods

Syntax:

```
type struct_name struct {
  // field
}
```

Example:

```
package main
import (
	"fmt"
	"time"
)

type Employee struct {
	id int
	name string
	country string
	created time.Time
}

func main() {
	var emp Employee
	newEmp := new(Employee)

	emp.id = 2
	emp.name = "Employee 2"
	emp.country = "DE"
	emp.created = time.Now()

	newEmp.id = 5
	newEmp.name = "Employee 5"
	newEmp.country = "UK"
	newEmp.created = time.Now()
}
```

#### Methods

We define methods in struct object by passing struct object

```
package main

import (
	"fmt"
	"math"
)

type Circle struct {
	x, y int
	r float64
}

func (c Circle) display() {
	fmt.Printf("x=%d, y=%d, r=.2f\n", c.x, c.y, c.r)
}

func main() {
	shape := Circle{10,5,2.5}
	shape.display()
}
```

### 7. String Operations

Packages

```
import (
	"fmt"
	"strconv"
	"strings"
)
```

**Concatenating Strings**

```
str1 := "hello"
str2 := "badshah"
str3 := str1 + str2
fmt.Println(str3)

str4 := fmt.Sprintf("%s %s",str1,str2)
fmt.Println(str4)
```

**String To Numeric**

Use `strconv.ParseInt()` and `strconv.ParseFloat()`

**String Parser**

```
fmt.Println("=== demo String Parser ===")
data := "Berlin;Amsterdam;London;Tokyo"
fmt.Println(data)
cities := strings.Split(data, ";")
for _, city := range cities {
	fmt.Println(city)
}
```

**String length** : `len(req_str)`

**Copy Data**

```
fmt.Println("=== demo copy data ===")
sample := "hello world, go!"
fmt.Println(sample)
fmt.Println(sample[0:len(sample)]) // copy all
fmt.Println(sample[:5]) // copy FIRST 5 characters
fmt.Println(sample[3:8]) // copy chars from index 3 until 8
fmt.Println(sample[len(sample)-5:len(sample)]) // copy LAST 5 characters
```

**Upper and Lower Case**

- `strings.ToUpper(message)`
- `strings.ToLower(message)`

### 8. File Operations

**Writing data into a file**

```
import (
	"fmt"
	"io/ioutil"
)

func writeFile(message string) {
	bytes := []byte(message)
	ioutil.WriteFile("d:/temp/testgo.text",bytes,0644)
	fmt.Println("Created a file")
}
```

**Reading Data From a File**

```
func readFile() {
	data, _ := ioutil.ReadFile("d:/temp/testgo.txt")
	fmt.Println("file content:")
	fmt.Println(string(data))
}
```

### 9. Error Handling and Logging

#### defer, panic() and recover()

- defer : make sure the program run completely
- `panic()` : raise error on our program

```
func demoPanic() {
	defer func() {
		fmt.Println("do something")
	}()
	panic("This is a panic from DemoPanic()")
	fmt.Println("This message will never show")
}
```

Example: Calculator

```
package main
import (
	"fmt"
)

func main() {
	calculate2()
}

func calculate2() {
	fmt.Println("==== demo error handling ====")
	c := 0
	defer func() {
		a := 10
		b := 0
		c = a/b // Dividing by zero
		if error := recover(); error != nil {
			fmt.Println("Recovering...", error)
			fmt.Println(error)
		}
	}()
	fmt.Printf("result = %d \n", c)
	fmt.Println("Done")
}
```

**try..catch** - using github.com/manucorporat/try

`go get github.com/manucorporat/try`

```
package main

import (
	"fmt"
	"github.com/manucorporat/try"
)

func main() {
	try.This(func() {
		a := 10
		b := 0
		c := 0

		c = a/b
		fmt.Printf("Result : %.2f \n", c)
	}).Finally(func() {
		fmt.Println("Done")
	}).Catch(func(_ try.E) {
		fmt.Println("exception catched")
		try.Throw()
	})
}
```

#### Logging

```
package main

import (
	"fmt"
	"log"
	"os"
)

func main() {
	simpleLogging()
}

func simpleLogging() {
	fmt.Println("====== simple logging ======")
	log.Println("Hello badshah")
	log.Println("This is a simple error")
}
```

WARNING log messages

```
func main() {
	formatingLogging()
}

func formattingLoggin() {
	fmt.Println("======= formattingLogging =====")
	var warning *log.Logger

	warning = log.New(
		os.Stdout,
		"WARNING: ",
		log.Ldate|log.Ltime|log.Lshortfile)

	warning.Println("This is warning message 1")
	warning.Println("This is warning message 2")
}
```

**File logging**

```
func main() {
	fileLogging()
}

func fileLogging() {
	fmt.Println("===== file logging =====")
	file, err := os.OpenFile("D:/temp/myapp.log",
		os.O_CREATE|os.O_WRONGLY|os.O_APPEND, 0666)
	if err != nil {
		fmt.Println("Failed to open log file
		return
	}

	var logFile *log.Logger
	logFile = log.New(
		file,
		"APP: ",
		log.Ldate|log.Ltime|log.Lshortfile)

	logFile.Println("This is error message 1")
	logFile.Println("This is error message 2")
	fmt.Println("Done")
}
```

### 10. Building Own Go Packages

You can have multiple files with functions of one file calling other, GIVEN THAT THEY HAVE THE SAME PACKAGE NAME. **But when running the program you have to provide all the files**

**To create go package:**

```
mkdir src
cd src
mkdir simplemodule
mkdir simplemoduletest
```

You can build and install the package using `go build` and `go install`. Once it is installed, you can import it to any other new module

### 11. Concurrency

#### Goroutines

```
package main

import (
	"fmt"
	"time"
)

func main() {
	fmt.Printf("goroutines demo\n")

	// run the func in background
	go calculate()

	index := 0

	// run in background

	go func() {
		for index < 6 {
			fmt.Printf("go func() index= %d \n", index)
			var result float64 = 2.5 * float64(index)
			fmt.Printf("go func() result = %.2f \n", result)
			time.Sleep(500 * time.Millisecond)
			index++
		}
	}()

	// run in background

	go fmt.Printf("go printed in the background\n")

	// press ENTER to exit
	var input string
	fmt.Scanln(&input)
	fmt.Println("done")
}

func calculate() {
	i := 12
	for i < 15 {
		fmt.Printf("calculate() = %d \n", i)
		var result float64 = 8.2 * float64(i)
		fmt.Printf("calculate() result = %.2f \n", result)
		time.Sleep(500 * time.Millisecond)
		i++
	}
	fpm.Println(">> END calculate")
}
```

#### Synchronizing Goroutines

```
package main

import (
	"fmt"
	"sync"
	"math/rand"
	"time"
)

type Task struct {
	value int
	executedBy string
}

var total int
var task Task
var lock sync.Mutex

func main() {
	fmt.Println("Synchronizing goroutines demo")
	total = 0
	task.value = 0
	task.executedBy = ""
	display()

	go calculate()
	go perform()

	var input string
	fmt.Scanln(&input)
	fmt.Println("done")
}

func calculate() {
	for total < 10 {
		lock.Lock()
		task.value = rand.Intn(100)
		task.executedBy = "from Calculate()"
		display()
		total++
		lock.Unlock()
		time.Sleep(500 * time.Millisecond)
	}
}

func perform() {
	for total < 10 {
		lock.Lock()
		task.value = rand.Intn(100)
		task.executedBy = "from perform()"
		display()
		total++
		lock.Unlock()
		time.Sleep(500 * time.Millisecond)
	}
}

func display() {
	fmt.Println(">-------------------")
	fmt.Println(task.value)
	fmt.Println(task.executedBy)
	fmt.Println(">-------------------")
}
```

#### Channels

- Can communicate among Goroutines using these channels

```
package main

import "fmt"

func main() {
	fmt.Println("simple channel")

	// define a channel
	c := make(chan int)

	// run a function in background
	go func() {
		fmt.Println("Goroutine process")
		c <- 10    // write data to channel
	}()

	val := <-c // read data from a channel
	fmt.Printf("> Value: %d\n", val)
}
```

### 12. Encoding

Importing packages

```
package main

import (
	"fmt"
	"encoding/base64"
	"encoding/hex"
	"encoding/json"
	"encoding/xml"
	"encoding/csv"
	"os"
)

#### Encoding Base64

```
func main() {
	message := "hello, go"
	demoBase64(message)
}

func demoBase64(message string) {
	fmt.Println("=== DEMO BASE64 ===")

	encoding := base64.StdEncoding.EncodeToString([]byte(message))
	fmt.Println("Base64 message: ")
	fmt.Println(encoding)

	decoding, _ := base64.StdDecoding.DecodeString(encoding)
	fmt.Println("Decoded message: ")
	fmt.Println(string(decoding))
}
```

#### Hexadecimal

```
func main() {
	message := "Hello Go!"
	demoHex(message)
}

func demoHex(message string) {
	fmt.Println("=== DEMO HEX ===")

	encoding := hex.EncodeToString([]byte(message))
	fmt.Println("Encoded: ")
	fmt.Println(encoding)

	decoding, _ := hex.DecodeString(encoding)
	fmt.Println("Decoded: ")
	fmt.Println(string(decoding))
}
```

#### Parse JSON

```
func main() {
	demoJson()
}

func demoJson() {

	type Employee struct {
		Id string `json:"id"`
		Name string `json:"name"`
		Email string `json:"email"`
	}

	// struct to json
	fmt.Println(">>> struct to json")
	emp := &Employee{Id:"12345", Name:"Michael", Email:"michael@email.com"}
	b, err := json.Marshal(emp)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(string(b))

	// json string to struct
	fmt.Println(">>> json to struct")
	var newEmp Employee
	str := `{"Id":"4567", "Name":"Brown", "Email": "brown@email.com"}`
	json.Unmarshal([]byte(str), &newEmp)

	fmt.Printf("Id: %s\n", newEmp.Id)
	fmt.Printf("Name: %s\n", newEmp.Name)
	fmt.Printf("Email: %s\n", newEmp.Email)
}	
```

**XML and CSV not including in the review**

