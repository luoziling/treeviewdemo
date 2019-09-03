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
