**给url设置参数

    ```js
        function changeURLPar(uri, par, par_value) {
            var pattern = par + '=([^&]*)';
            var replaceText = par + '=' + par_value;
            if (uri.match(pattern)) { //如果连接中带这个参数
                var tmp = '/\\' + par + '=[^&]*/';
                tmp = uri.replace(eval(tmp), replaceText);
                return (tmp);
            } else {
                if (uri.match('[\?]')) { //如果链接中不带这个参数但是有其他参数
                    return uri + '&' + replaceText;
                } else { //如果链接中没有带任何参数
                    return uri + '?' + replaceText;
                }
            }
            return uri + '\n' + par + '\n' + par_value;
        }

        function changeUrl() {

            var newurl = changeURLPar(window.location.href, 's', '000001'); //将uid和现有的页面地址拼接

            window.history.pushState(null, null, newurl); //向当前url添加参数
        }
        changeUrl()
    ```

**获取url参数

    ```js
        function getQueryVariable(variable) {
            var query = window.location.search.substring(1);
            var vars = query.split("&");
            for (var i = 0; i < vars.length; i++) {
                var pair = vars[i].split("=");
                if (pair[0] == variable) {
                    return pair[1];
                }
            }
            return ('000001');
        }
    ```