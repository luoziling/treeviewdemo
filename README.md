# Bootstrap TreeView Demo

## 这是一个bootstrap treeview的demo

**本项目通过springboot整合bootstrap实现从前台通过ajax向后台发送请求，后台通过通过字符拼接返回json数组给前台，前台再通过字符拼接结合TreeView实现树形控件的显示。**

### 1.TreeView实现单点收起展示的小技巧

这个就直接给链接了原作者写的很好

给bootstrap-treeview增加点击事件，单击菜单也能展开和折叠

<https://blog.csdn.net/zsg88/article/details/50805800>

**给点小提示：**

在修改完bootstrap-treeview.js的源码之后第二部实现的onClick方法要在自己需要使用树形控件的html网页中通过script的方式添加

例如：

```javascript
<script type="text/javascript">

    function itemOnclick(target){
        //找到当前节点id
        var nodeid = $(target).attr('data-nodeid');
        var tree = $('#tree');
        //获取当前节点对象
        var node = tree.treeview('getNode', nodeid);

        if(node.state.expanded){
            //处于展开状态则折叠
            tree.treeview('collapseNode', node.nodeId);
        } else {
            //展开
            tree.treeview('expandNode', node.nodeId);
        }
    }

</script>
```

### 2.搭建环境导入html需要的css以及js

```html
<!-- Required Stylesheets -->
<link href="css/bootstrap.css" rel="stylesheet" />
<link href="css/bootstrap-treeview.css" rel="stylesheet" />

<!-- Required Javascript -->
<script src="js/jquery-3.4.1.js"></script>
<script src="js/bootstrap-treeview.js"></script>
<script src="js/bootstrap.min.js"></script>
```

### 3.编写ajax向后台发送请求获取数据（拼接的字符串

```javascript
function getTreeJson(){
    $.ajax({
        type: "GET",
        url: "/getData",
        data: null,
        dataType: "text",
    success: function(data){
        var dt = [{
            text: '标题',
            nodes: eval('[' + data +']')
        }]

        $('#tree').treeview({
            data: dt
        })
    },
    error: function(XMLHttpRequest){
        alert(XMLHttpRequest.status);
    }
});
}
```

同时在html页面中添加id为tree的div

```html
<div class="container">
    <div class="row" >
        <div class="col-sm-6 col-md-offset-3" id="tree"></div>
        <!-- <div class="col-sm-6" id="aa"></div> -->
    </div>
</div>
```

### 4.后台实现

通过RESTful风格的请求返回所有对应的的html网页，以及获取数据

```java
package com.wzb.treeviewdemo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

/**
 * @author Satsuki
 * @time 2019/9/3 21:07
 * @description:
 */
@Controller
public class TestController {

    @RequestMapping("/{page}")
    public String showPage(@PathVariable String page){
        return page;
    }

    @RequestMapping("/getData")
    @ResponseBody
    public String  getData(){
        String res = getDataString();
        System.out.println(res);
        return res;
    }

    public String getDataString(){
        StringBuilder nodesData = new StringBuilder("");

        //后续需要参考android端的取数据的实现
        //这里先简易的通过自己的手写拼字段返回
        nodesData.append("{text: 'Parent 1',nodes: [{text: 'Child1',nodes: [{text: 'Grandchild1'},{text: 'Grandchild2'}]},{text: 'Child2'}]}," +
                "{text: 'Parent 2'}," +
                "{text: 'Parent 3'}," +
                "{text: 'Parent 4'}," +
                "{text: 'Parent 5'}"
        );


        return nodesData.toString();
    }
}
```

