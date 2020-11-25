# Useful methods in Arrays

## Arrays

- array will have Array as _proto_

    ![Useful%20methods%20in%20Arrays%202c59a51acad74da095ca8ab9bd21ce98/Untitled.png](Useful%20methods%20in%20Arrays%202c59a51acad74da095ca8ab9bd21ce98/Untitled.png)

- otherwise, it is an array-like object.

    ![Useful%20methods%20in%20Arrays%202c59a51acad74da095ca8ab9bd21ce98/Untitled%201.png](Useful%20methods%20in%20Arrays%202c59a51acad74da095ca8ab9bd21ce98/Untitled%201.png)

- A collection of variables which can have different data types.

```jsx
// Initialize new array.
var names = ['a', 'b', 'c'];
var years = new Array(1990, 2000, 2010);

// different types
var i = ['allen', 25, true];

// mutate the array
names[1] = 'S';
// ['S','b','c'];

names.push('') // add the value to the end of the array, return **length** property
names.unshift('') // add the value to the start of the array, return **length**
names.pop() // remove the last element. return the removed element
names.shift() // remove the first element. return the removed element
names.indexOf('a') // return the index. if there is no the matched value in the array, 
										//**will reutrn -1**
```

### slice() - Get part of an Array or Convert A List To An Array

**Return the sliced item(s) to a new array**

- we can use the `slice(start, end)` method - **[start, end)**, which is a build-in method in Array.
    - The `slice()` method **returns** the selected elements in an array, as a new array object.
    - The `slice()` method selects the elements starting at the given *`start`* argument, and ends at, ***but does not include*, the given *`end`* argument.**
    - The `slice()` method will return the whole array into a new array if start and end are **NOT** given.
    - **Note:** The original array will not be changed.

**Convert List to Array**

- We cannot directly do something like this → `aList.slice()`
- Because `slice()` belongs to Array object, so we can access it by calling **`Array`** and we need to perform **method borrowing**
    - `var fieldsArr = Array.prototype.slice.call(fields);`

    ```jsx
    function list() {
      return Array.prototype.slice.call(arguments)
    }

    let list1 = list(1, 2, 3) // [1, 2, 3]
    ```

### splice() - Add or Delete Item From Array

`let arrDeletedItems = array.splice(start[, deleteCount[, item1[, item2[, ...]]]])`

- **Return the removed item(s) and mutate the original array.**
- `array.splice(index, 1);`
    - **Remove** `1` elements at the `index`.
    - `index`: An integer that specifies at what position to add/remove items, Use negative values to specify the position from the end of the array.
        - starting from the index to perform the action.
    - 1: the number of items you want to delete.
- `array.splice(index, 0, item);`
    - **Add** `item` before the `index` position.
- `array.splice(index, 1, item);`
    - **Replace** `item` at the `index` position

```jsx
// Only delete the first matching element
function removeItemOnce(arr, value) {
  let index = arr.indexOf(value);
  if (index > -1) {
    arr.splice(index, 1);
  }
  return arr;
}

// Delete all matching cases.
function removeItemAll(arr, value) {
  let i = 0;
  while(i < arr.length) {
    if (arr[i] === value) {
      arr.splice(i, 1); // remove the element from array at index i.
    } else {
      i++;
    }
  }
}
```

- **NOTE: `splice()` will modify the array and the index of the rest elements of the array will be changed as well. In case to remain the same index, use `delete` operator instead:**

    `delete expression`

    - Return `true` else `false` to indicate the successful deletion

    ```jsx
    const array = [1, 2, 3, 4];

    console.log(array);
    // expected output: [1, 2, 3, 4]

    delete array[2];

    console.log(array);
    // expected output: [1, 2, undefined, 4]
    ```

### concat() - Merge two or more arrays

`const new_array = old_array.concat([value1[, value2[, ...[, valueN]]]])`

- return a new array.
- Does not change the existing arrays
- `const newArray = array1.concat(array2, array3, ...);`
- Concatenating values to an array
    - `const newArray = array1.concat(1, [2, 3]);`

### join() - Join String Items Together

- `arr.join([separator])`
    - It will concatenate all elements in the array, including sub-array in the array.
    - By default, separator is `,`
    - `separator` is optional
    - Return a new string
    - If the array only contains one element, return that element without the separator.