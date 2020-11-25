# Bootstrap

# Import Bootstrap

- Add stylesheet into HTML file

    ```html
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">
    ```

- Add JS files if we need any click events like button, navigation button etc.
    - If we only need its style, we don't need to import script files.

    ```html
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ho+j7jyWK8fNQe+A12Hb8AhRq26LrZ/JpcUGGOn+Y7RsweNrtN/tE3MoK7ZeZDyx" crossorigin="anonymous"></script>
    ```

# Bootstrap Columns

- Bootstrap提供各种模版
- Bootstrap设定container 为12个空间，我们可以直接通过设置class名字来create 占据不同空间的column。
- 最有用的功能就是我们可以通过添加不同的class，让bootstrap自己通过判断窗口大小来安排column的位置。
    - 在window size 为small时，column占比为6 3 3
    - 在window size 为extra large时，column占比为12（自己一行），6 6（剩下两个div 平分第二行）。

    ```html
    <div class="container">
        <div class="row">
            <div class="col col-sm-6 col-xl-12">
                One of three columns
            </div>
            <div class="col col-sm-3 col-xl-6">
                One of three columns
            </div>
            <div class="col col-sm-3 col-xl-6">
                One of three columns
            </div>
        </div>
    </div>
    ```