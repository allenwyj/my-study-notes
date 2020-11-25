# Integrate multiple validation string in one annotation.

Integrate multiple validation string in one annotation. For example, validate against both LUV and TOPS.

**Answer:**

Yes, you can do this. In your validation, you will make use of an array of strings.

Here's an overview of the steps.

1. Update CourseCode.java to use an array of strings

2. Update CourseCodeConstraintValidator.java to validate against array of strings

3. Update Customer.java to validate using array of strings

---

**Detailed Steps**

**1. Update CourseCode.java to use an array of strings**

Change the value entry to an array of Strings:

// define default course code public String[] value() default {"LUV"};

Note the use of square brackets for the array of Strings. Also, the initialized value uses curley-braces for array data.

**2. Update CourseCodeConstraintValidator.java to validate against array of strings**

Change the field for coursePrefixes to an array

`private String[] coursePrefixes;` 

Update the isValid(...) method to loop through the course prefixes. In the loop, check to see if the code matches any of the course prefixes.

@Override public boolean isValid(String theCode,  ConstraintValidatorContext theConstraintValidatorContext) { boolean result = false;  if (theCode != null) {  // // loop thru course prefixes // // check to see if code matches any of the course prefixes // for (String tempPrefix : coursePrefixes) { result = theCode.startsWith(tempPrefix);  // if we found a match then break out of the loop if (result) { break; } } } else { result = true; }  return result; }

**3. Update Customer.java to validate using array of strings**

@CourseCode(value={"TOPS", "LUV"}, message="must start with TOPS or LUV") private String courseCode;

Note the use of curley braces.

---

Complete Source Code:

[https://gist.github.com/darbyluv2code/0275ddb6e70e085a10fd464e36a42739](https://gist.github.com/darbyluv2code/0275ddb6e70e085a10fd464e36a42739)

---

That's it. This provides a solution to integrate multiple validation string in one annotation. In this example, the code validates against both LUV and TOPS.