The `range` keyword in Go is used in **`for` loops to iterate over elements in a variety of data structures**. These structures include arrays, slices, strings, maps, and channels. The `for-range` statement provides a concise and readable way to access the elements of these collections.

When you use `range`, the loop iterates through the data structure, and for each element, it provides you with **one or two values**:

- For **arrays, slices, and strings**, `range` provides the **index** of the element as the first value (integer) and a **copy of the element** at that index as the second value.
- For **maps**, `range` provides the **key** of the element as the first value and the **value** associated with that key as the second value. The **iteration order for maps is not guaranteed**.
- For **channels**, `range` provides only a **single value**, which is the next value received from the channel. The loop continues until the channel is closed.

If you only need one of the values (either the index/key or the value), you can use the **blank identifier `_` to discard the unwanted value**. This is a common idiom in Go.

It's important to note that when iterating over arrays, slices, and strings, the **second value provided by `range` is a copy of the element**. Modifying this copied value within the loop will not change the original element in the data structure.

Starting with Go 1.22, when the `go` directive in the `go.mod` file is set to `1.22` or higher, a `for` loop creates a **new index and value variable on each iteration**. In earlier versions, the same variable was reused across iterations, which could lead to issues with closures in goroutines.

Here's a code example demonstrating the use of `range` with different data types:

```go
package main

import "fmt"

func main() {
	// Range over a slice
	numbers := []int{10, 20, 30}
	fmt.Println("Iterating over a slice:")
	for index, value := range numbers {
		fmt.Printf("Index: %d, Value: %d\n", index, value)
		value *= 2 // This modifies the copy, not the original slice element
	}
	fmt.Println("Original slice:", numbers)

	fmt.Println("\nIterating over a slice (ignoring index):")
	sum := 0
	for _, value := range numbers {
		sum += value
	}
	fmt.Println("Sum:", sum)

	// Range over an array
	primes :=int{2, 3, 5, 7, 11}
	fmt.Println("\nIterating over an array:")
	for index, prime := range primes {
		fmt.Printf("Index: %d, Prime: %d\n", index, prime)
	}

	// Range over a string (iterating over runes)
	message := "你好 Go"
	fmt.Println("\nIterating over a string:")
	for index, char := range message {
		fmt.Printf("Index: %d, Character: %c (Unicode code point: %U)\n", index, char, char)
	}

	// Range over a map
	ages := map[string]int{"Alice": 30, "Bob": 25}
	fmt.Println("\nIterating over a map:")
	for name, age := range ages {
		fmt.Printf("Name: %s, Age: %d\n", name, age)
	}

	fmt.Println("\nIterating over a map (ignoring value):")
	for key := range ages {
		fmt.Println("Key:", key)
	}

	// Note: Ranging over channels is typically done in goroutines
	// and continues until the channel is closed.
}
```

In summary, the `for-range` loop in Go is a versatile and idiomatic way to iterate over elements in various collection types, providing either one or two values representing the index/key and the value of each element. The blank identifier `_` allows you to easily ignore values you don't need, and it's crucial to remember that for most iterable types, `range` provides a copy of the element.