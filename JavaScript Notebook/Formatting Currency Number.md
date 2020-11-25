# Formatting Currency Number

### Formatting Number - Currency

- **regex -** `/(\d)(?=(\d{3})+(?!\d))/g`
    - `/.../` - indicates the start and the end of an regular expression and the second one also indicates the start of expression flag
    - `g` - retain the index of the last match, allowing iterative searches.
    - `\d` - Looking for a digit
    - `\d{3}` - Looking for three digits
    - `+` - the expression in front of `+` can have one or more.
    - Positive Lookahead  `(?=)`
        - `A(?=B)`

            Find the matching **position** where expression A is immediately followed by expression B 

    - Negative Lookahead `(?!)`
        - `A(?!B)`

            Find the matching **position** where expression A is **NOT** immediately followed by expression B 

    - `(?=(\d{3})` - Looking for a position where it is immediately followed by three digits.
    - `(?!\d)` - Looking for a position where it doesn't immediately have digit following.
    - `(?=(\d{3})+(?!\d))` -  Looking for a position where it has one or more sets (3 digits per set) and end up with non-digit.
    - `(\d)(?=(\d{3})+(?!\d))` - Looking for a digit which has a position where it is followed by  one or more 3-digits sets, and then followed by a position where it ends up with non-digit.
    - For example, if we have a string 'ad12345678abs', this regex will match the positions at 2[the matched position]3, and 5[the matched position]6
- **NOTE: This regex will also handle floats which is smaller than 1.**
- Code

    ```jsx
    const formatNumber = function(num, type) {
      var numSplit, int, dec;

      // returning an absolute number
      num = Math.abs(num);
      // giving 2 decimal numbers and return its value as a string
      num = num.toFixed(2);

      numSplit = num.split('.');
      int = numSplit[0];

      // adding thousand seperator
      // if(int.length>3){
      //   int = int.substr(0,int.length-3)+','+int.substr(int.length-3,3);
      // }

    	// $1 - (\d): the matched digit
    	// $2 - (?=(\d{3})+(?!\d))
      int = int.replace(/(\d)(?=(\d{3})+(?!\d))/g,'$1,');

      dec = numSplit[1];

      // adding '+' or '-' 
      type === 'exp' ? sign = '-' : sign = '+';

      return sign + ' ' + int + '.' + dec;
    };
    ```

- Test

    ![Formatting%20Currency%20Number%20591c85feca6a4666ba053d4faa2f64b7/Untitled.png](Formatting%20Currency%20Number%20591c85feca6a4666ba053d4faa2f64b7/Untitled.png)