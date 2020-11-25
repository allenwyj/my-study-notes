# 返回按钮

1. 可以用`window.history.back`

    ```tsx
    const onClickBack = () => {
    	window.history.back();
    }
    ```

    - 不会让 react 重新刷新页面
        - 可通过查看开发者工具中 network，查看 请求发送
    - 存在一个Bug，如果用户是通过复制链接然后进入的页面，此时点击返回按钮，他会回到浏览器之前的页面，而不是App 中当前页面的上一页面。可解决，较难。
2. 推荐方法：可以用 React 封装好的history back方法。

    ```tsx
    import { useHistory } from 'react-router-dom';

    ...
    const history = useHistory();

    const onClickBack = () => {
      history.goBack();
    };
    ```