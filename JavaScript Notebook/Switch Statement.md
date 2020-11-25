# Switch Statement

### Switch Statement

- **If you omit (省略) the `break` statement, the next case will be executed even if the evaluation does not match the case.**
    - We can use this to process the same statement with different cases.

    ```jsx
    let job = 'uni lecturer';
    switch(job) {
    	case 'uni lecturer':
    	case 'uni tutor': // this case will execute no matter it is uni tutor or not.
    		// do something
    		break;
    	default:
    		// do something
    }
    ```

- If `default` is not the last case in the switch block, remember to end the default case with a `break`.
- If we are using `return` statement, then we dont need to use `break`.