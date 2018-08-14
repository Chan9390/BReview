## 3. Basic Data Types

### 3.1 Integers

- 4 signed : `int8`, `int16`, `int32`, `int64`
- 4 unsigned : `uint8`, `uint16`, `uint32`, `uint64`
- NOTE: 2 variables of the same type are comparable using `==` and `!=`

### 3.2. Floating Point numbers

### 3.3 Complex Numbers

- `complex64` and `complex128`
- Ex: `var x complex128 = complex(1,2)` (1+2i)

### 3.4 Booleans

- Type `bool`, values `true` and `false`

### 3.5 Strings

- Strings : immutable sequence of bytes
- The i-th byte of a string is not necessarily the i-th character of the string, because UTF-8 encoding of a non-ASCII code point requires 2 or more bytes
- **String Literals**, **Unicode**, **UTF-8**, **Strings & Byte Slices**
- **Conversions between Strings and Numbers**
  - To convert integer to string, one option is to use `fmt.Sprintf`
  - `strconv.Itoa()`
  - String to int: `strconv.Atoi()` or `strconv.ParseInt("123", 10, 64)` // base 10 upto 64 bits

### 3.6 Constants

- Declaration: `const`
- Computations on constants are evaluated at compile time so that it saves time during run time

#### The constant generator `iota`

- In a const declaration, the value of `iota` begins at zero and increments by one for each time in the sequence. (More like enum)

```go
type Weekday int

const (
  Sunday Weekday = iota
  Monday
  Tuesday
  Wednesday
  Thursday
  Friday
  Saturday
)
```

- The above code declares sunday = 0 to sat = 6

- We can use `iota` for more complex tasks

```go
type Flags uint

const (
  FlagUp Flags = 1 << iota
  FlagBroadCast
  FlagLoopback
  FlagPointToPoint
  FlagMulticast
)
```

#### Untyped constants

- 6 flavors of uncommitted constants:
  - untyped boolean
  - untyped integer
  - untyped rune
  - untyped floating-point
  - untyped complex
  - untyped string