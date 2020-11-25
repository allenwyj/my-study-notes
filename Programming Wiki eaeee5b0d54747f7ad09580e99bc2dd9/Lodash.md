# Lodash

# Why Lodash?

Lodash makes JavaScript easier by taking the hassle out of working with arrays, numbers, objects, strings, etc.Lodash’s modular methods are great for:

- Iterating arrays, objects, & strings
- Manipulating & testing values
- Creating composite functions

# Lodash Methods

### _.debounce

`_.debounce(func, [wait=0], [options={}])`

Creates a debounced function that **delays** invoking **func** until after **wait** **milliseconds** have elapsed since the last time the debounced function was invoked. The debounced function comes with a cancel method to **cancel delayed func** invocations and a **flush** method to immediately invoke them. Provide options to indicate whether func should be invoked on the **leading** and/or **trailing** edge of the wait timeout. The func is invoked with the last arguments provided to the debounced function. Subsequent calls to the debounced function return the result of the last func invocation.