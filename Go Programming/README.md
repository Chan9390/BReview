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
