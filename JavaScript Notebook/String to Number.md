# String to Number

### Convert String to Number

- `parseInt(string [, radix])` - Convert to number
    - **always specify the radix.**
    - If the radix parameter is omitted, JavaScript assumes the following:
        - If the string begins with "0x", the radix is 16 (hexadecimal)
        - If the string begins with "0", the radix is 8 (octal). This feature is deprecated
        - If the string begins with any other value, the radix is 10 (decimal)
    - **Note:** Only the first number in the string is returned!
    - **Note:** Leading and trailing spaces are allowed.
    - **Note:** If the first character cannot be converted to a number, parseInt() returns NaN.
- `parseFloat()` - Convert to float