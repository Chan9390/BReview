### Preface

- Go has automatic memory management or garbage collection
- Runs faster than programs in other programming languages and suffer fewer crashes due to unexpected type errors
- Tony Hoare's seminal 1978 paper - **communicating sequential processes** - a program is a parallel composition of processes that have no shared state; the process communicate and synchronize using channels.

### The Go Project

- Go lang
  - garbage collection
  - package system
  - first-class functions
  - lexical scope
  - a system call interface
  - immutable strings in which the text is generally encoded in UTF-8
- **Golang doesn't have:**
  - implicit numeric conversions
  - constructors or destructors
  - operator overloading
  - default parameter values
  - inheritence
  - generics
  - exceptions
  - macros
  - function annotations
  - thread-level storage
- Go's aggregate types: structs and array
- Go version : `go version`

## 1. Tutorial

### 1.1 Hello world

```
package main

import "fmt"

func main() {
    fmt.Println("Hello tester")
}
```
- Go is a compiled language. It natively handles Unicode.
- Go code organized in packages
- The code should **start with the name of the package**.
- `package main` : defines a standalone executable program, not a library
- A program will not compile if there are missing / unnecessary imported packages
- Doesnt require semicolons `;` at the end unless you put together multiple statements
- `gofmt` - rewrites the code into standard format
- `goimports` - manages the insertion / removal of import declaration

### 1.2 Command-line arguments

- `os` package - platform independent. To get commandline args: `os.Args` - its a slice of strings. No. of elements : `len(s)`
- To get all the arguments: `os.Args[1:len(os.Args)]` or `os.Args[1:]`

Example of `echo` program written in Go:

```
// Echo1 prints its command-line arguments
package main

import (
    "fmt"
    "os"
)

func main() {
    var s, sep string

    for i := 1; i < len(os.Args); i++ {
        s += sep + os.Args[i]
        sep = " "
    }
    fmt.Println(s)
}
```

- If variables not explicitly initialized, then they are equal to 0 or ""
- `:=` short variable declaration, a statement that declare one or more variables and gives them appropriate types based on the initializer values
- `j = i++` is illegal and `--i` is also not used in Go

```
// A traditional while loop
for condition {
    // do something
}
```

```
// A traditional infinite loop
for {
    // do something
}
```

- Another form of for loop iterates over a range of values from data type like a string or a slice

```
for _, arg := range os.Args[1:] {
    s += sep + arg
    sep = " "
}
```

- The **range provides a pair of values** : the index and the value of the element at that index
- As go doesnt allow the use of unused local variables, the solution is to use **blank identifier** (`_`). This is used whenever syntax requires a variable name but program logic doesnt

- Several ways to declare a variable
  - `s := ""`
  - `var s string`
  - `var s = ""`
  - `var s string = ""`
- Short variable declaration is compact but used only within a function, not a package-level variables

- One can concatinate strings using the `strings.Join()`. Example:

```
func main() {
    fmt.Println(strings.Join(os.Args[1:], " "))
}
```

### 1.3 Finding Duplicate Lines

```
// Dup1 prints the text of each line that appears more than once in the standard output, preceded by its count.

package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    counts := make(map[string]int)
    input := bufio.NewScanner(os.Stdin)
    for input.Scan() {
        counts[input.Text()]++
    }

    // NOTE: ignoring potential errors from input.Err()
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
```

- **Map** holds a set of key/value pairs and provide constant-time operations to store, retrieve or test for an item in the set. The key can be of any type whose values can be compared with `==`. (Strings are common)
- **`bufio` package** : makes input and output efficient. `Scanner`: reads input and breaks it into lines or words. `input.Scan()` - reads next line and removes the newline char. Returns true if there is a line, else returns false
- Formatter : `%d` for int (decimal operand), `%s` for string

Formatters:

| Symbol | Used for |
|----|----|
| `%d` | decimal integer |
| `%x` `%o` `%b` | integer to hexadecimal, octal, binary |
| `%f` `%g` `%e` | floating-point number: 3.141414, 3.141414144141414, 3.141414e+00 |
| `%t` | boolean: true or false |
| `%s` | string |
| `%v` | any value in a natural format |
| `%T` | type of any value |

- Any function which ends with f supports formatting: `log.Printf`, `fmt.Errorf`, `fmt.Printf`

```
// Dup2 prints the count and text of the lines that appear more than once in the input.
// It reads from the standard input  or from the list of the named files.

package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    counts := make(map[string]int)
    files := os.Args[1:]
    if len(files) > 0 {
        countLines(os.Stdin, counts)
    } else {
        for _, arg := range files {
            f, err := os.Open(arg)
            if err != nil {
                fmt.Printf(os.Stderr, "dup2: %v\n", err)
                continue
            }
            countLines(f, counts)
            f.Close()
        }
    }

    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}

func countLines(f *os.File, counts map[string]int) {
    input := bufio.NewScanner(f)
    for input.Scan() {
        counts[input.Text()]++
    }
    // NOTE: ignoring potential errors from the input.Err()
}
```

- `map` is a reference to the data structure created by `make`. When a map is passed to a function, the function receives a copy of the reference, so any changes the called function makes to the data structure will be visible through the caller's map reference too.
- The above *streaming mode* - the input is read and broken into lines as needed
- Entire input can be read into memory in one big gulp, and split it into lines all at once, then process the lines.
- The following program only gets input from files and uses `io/ioutil`

```
package main

import (
    "fmt"
    "io/ioutil"
    "os"
    "strings"
)

func main() {
    counts := make(map[string]int)
    for _, filename := range os.Args[1:] {
        data, err := ioutil.ReadFile(filename)
        if err != nil {
            fmt.Printf(os.Stderr, "dup3: %v\n", err)
            continue
        }
        for _, line := range strings.Split(string(data), "\n") {
            counts[line]++
        }
    }

    for line, n : range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
```

- `ReadFile` returns a byte slice that must be converted to string

### 1.4 Animated GIFs

**Not interested**

### 1.5 Fetching a URL

- `net` package. `net/http` for http connections

```
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
	"os"
)

func main() {
	for _, url := range os.Args[1:] {
		resp, err := http.Get(url)
		if err != nil {
			fmt.Fprintf(os.Stderr, "http-connect: %v\n", err)
			os.Exit(1)
		}
		b, err := ioutil.ReadAll(resp.Body)
		resp.Body.Close()
		if err != nil {
			fmt.Fprintf(os.Stderr, "http-connect: reading %s: %v\n", url, err)
			os.Exit(1)
		}
		fmt.Printf("%s", b)
    }
}

```

- `ioutil.ReadAll` reads the entire response

### 1.6 Fetching URLs concurrently

```
package main

import (
	"fmt"
	"io"
	"io/ioutil"
	"net/http"
	"os"
	"time"
)

func main() {
	start := time.Now()
	ch := make(chan string)
	for _, url := range os.Args[1:] {
		go fetch(url, ch) // start a goroutine
	}
	for range os.Args[1:] {
		fmt.Println(<-ch) // receive from channel ch
	}
	fmt.Printf("%.2fs elapsed\n", time.Since(start).Seconds())
}

func fetch(url string, ch chan<- string) {
	start := time.Now()
	resp, err := http.Get(url)
	if err != nil {
		ch <- fmt.Sprint(err) // send to channel ch
		return
	}
	nbytes, err := io.Copy(ioutil.Discard, resp.Body)
	resp.Body.Close() // dont leak resources
	if err != nil {
		ch <- fmt.Sprintf("While reading %s: %v", url, err)
		return
	}
	secs := time.Since(start).Seconds()
	ch <- fmt.Sprintf("%.2fs %7d %s", secs, nbytes, url)
}
```

- **goroutine** - concurrent function execution
- **channel** - communication mechanism that allows one goroutine to pass values of a specified type to another goroutine
- In the above example, the `go` statement starts a goroutine that calls fetch asynchronously to fetch the URL

### 1.7 A Web Server

- To handle all the URIs:

```
package main

import (
	"fmt"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", handler) // each request call handler
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "URL Path = %q\n", r.URL.Path)
}
```

- To handle specific URIs separately

```
package main

import (
	"fmt"
	"log"
	"net/http"
	"sync"
)

var mu sync.Mutex
var count int

func main() {
	http.HandleFunc("/", handler)
	http.HandleFunc("/count", counter)
	log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

func handler(w http.ResponseWriter, r *http.Request) {
	mu.Lock()
	count++
	mu.Unlock()
	fmt.Fprintf(w, "URL Path= %q\n", r.URL.Path)
}

func counter(w http.ResponseWriter, r *http.Request) {
	mu.Lock()
	fmt.Fprintf(w, "Count %d\n", count)
	mu.Unlock()
}
```

- To get the parameters of the HTTP requests

```
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "%s %s %s\n", r.Method, r.URL, r.Proto)
	for k, v := range r.Header {
		fmt.Fprintf(w, "Header[%q] = %q\n", k, v)
	}
	fmt.Fprintf(w, "Host = %q\n", r.Host)
	fmt.Fprintf(w, "RemoteAddr = %q\n", r.RemoteAddr)

	if err := r.ParseForm(); err != nil {
		log.Print(err)
	}
	for k, v := range r.Form {
		fmt.Fprintf(w, "Form[%q] = %q\n", k, v)
	}
}
```

### 1.8 Loose Ends

**Switch case**

- The result of `coinflip()` is compared to value of each case

```
switch coinflip() {
    case "heads":
        heads++
    case "tails":
        tails++
    default:
        fmt.Println("landed on edge!")
}
```

- ***Tagless switch***

```
func Signu(x int) int {
    switch {
        case x > 0:
            return +1
        default:
            return 0
        case x < 0:
            return -1
    }
}
```

**Named types**

```
type Point struct {
    X, Y int
}
var p Point
```

**Pointers** : Values that contain the address of a variable

- You can access the docs of a library using `go doc`. Ex: `go doc http.ListenAndServe`