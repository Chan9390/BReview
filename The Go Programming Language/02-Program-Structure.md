## 2. Program Structure

### 2.1 Names

- Names begin with a letter or an underscore and may have any number of additional letters, digits and underscores
- If the name begins with an upper-case letter, it is exported - visible and accessible outside its own package

### 2.2 Declarations

- 4 types: `var`, `const`, `type`, `func`

### 2.3 Variables

- Syntax: `var name type = value`
- In Go there's no such thing as an uninitialized variable
- Ex: `var b, f, s = true, 2.3, "four"`, `i, j := 0, 1`

**Pointers**

```
x := 1
p := &x
fmt.Println(*p)   // "1"
*p = 2            // equivalent to x = 2
fmt.Println(x)    // "2"
```

```
func incr(p *int) int {
    *p++ // increments what p points to; doesnt change p
    return *p
}

v := 1
incr(&v)              // v = 2
fmt.Println(incr(&v)) // v = 3
```

**The `new` function**

- The expression `new(T)` creates an unnamed variable of type T, initializes it to zero value of T, and returns its address, which is a value of type *T

```
p := new(int)   // p, of type *int, points to an unnamed int variable
fmt.Println(*p) // "0"
*p = 2
fmt.Println(*p) // "2"
```

- Each call to `new` returns a distinct variable with unique address
- NOTE: two variables whose type carries no information and is therefore of size zero, such as struct{} or [0]int, may, depending on implementation, have the same address

### 2.4 Assignments

```
x = 1                       // named variable
*p = true                   // indirect variable
person.name = "bob"         // struct field
count[x] = count[x] * scale // array or slice or map element
```

### 2.5 Type declaration

- Syntax: `type name underlying-type`
- A `type` declaration defines a new named type that has the same underlying type as an existing type.

```
// Package tempconv performs Celsius and Fahrenheit temp conversions
package tempconv

import "fmt"

type Celsius float64
type Fahrenheit float64

const (
    AbsoluteZeroC Celsius = -273.15
    FreezingC Celsius = 0
    BoilingC Celsius = 100
)

func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32)}
func FToC(f Fahrenheit) Celsius { return Celsius((f-32)*5/9)}
```

- In the above func, you can have any arithmetic ops between type Fahrenheit and Celsius even though both are float64. There should be explicit conversions.

```
func (c Celsius) String() string { return fmt.Sprintf("%g C", c)}

c := FToC(212.0)
fmt.Println(c.String()) // 100 C
```

### 2.6 Packages and Files

- To make variables / functions available outside the package, start with a capital letter

**Imports**: Try using `goimports`

**Package Initialization**: `init` function can't be called or referenced. It gets automatically executed. Initialization process starts from the bottom-up; the main package is the last to be initialized

### 2.7 Scope

- Scope of a declaration is the part of the source code where the use of a declared name refers to the declaration. Its a compile time property. (Lifetime is a run time property)
- A program may contain multiple declarations of the same name so long as each declaration is in a different lexical block

```golang
func main() {
    x := "hello"
    for _, x := range x {
        x := x + 'A' - 'a'
        fmt.Printf("%c", x)  // "HELLO" (one letter per iteration)
    }
}
```