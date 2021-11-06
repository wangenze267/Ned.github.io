---
title: 对el-table-column的简单封装
author: Ned
tag:
  - Vue
abbrlink: 82139994
---

## 前言
最近在写一个练手项目，接触到了一个问题，就是`el-table`中的项太多了，我写了一堆`el-table-column`，导致代码太长了，看起来特别费劲，后来发现了一个让人眼前一亮的方法，瞬间挽救了我的眼睛。

下面就来分享一下！
## 进入正题

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5f18a5d8bbfd4eeca413208315f79572~tplv-k3u1fbpfcp-watermark.image?)
上面就是table中的全部项，去除第一个复选框，最后一个操作的插槽，一共七项，也就是说`el-table-column`一共要写9对。这简直不能忍！

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8c4696a1cdb64e0b9c5a80e959f9b0e4~tplv-k3u1fbpfcp-watermark.image?)
>这个图只作举一个例子用，跟上面不产生对应关系。

其中就有5个`el-form-item`，就这么一大堆。

所以，我当时就想，可不可以用`v-for`去渲染`el-table-column`这个标签呢？保留复选框和最后的操作插槽，我们只需要渲染中间的那几项就行。

经过我的实验，确实是可以实现的。
> 这么写之后就开始质疑之前的我为什么没有这个想法? 要不就能少写一堆💩啦
> **实现代码如下（标签部分）：**
```Vue.js
<el-table-column
            v-for="item in columns"
            :key="item.prop"
            :prop="item.prop"
            :label="item.label"
            :formatter="item.formatter"
            :width="item.width">
</el-table-column>
```
思路是这样，把标签需要显示的定义在一个数组中，遍历数组来达到我们想要的效果，`formatter`是我们完成提交的数据和页面显示数据的一个转换所用到的。具体写法在下面**js**部分有写。

定义数组的写法是**vue3 composition api**的写法，这个思路的话，用Vue2的写法也能实现的，重要的毕竟是思想（啊，我之前还是想不到这种思路）。
> 再吐槽一下下，这种写法每写一个函数或者变量就要return回去，也挺麻烦的感觉，hhhhh

**实现代码如下（JS部分）：**
```Vue.js
const columns = reactive([
      {
        label:'用户ID',
        prop:'userId'
      },
      {
        label:'用户名',
        prop:'userName'
      },
      {
        label:'用户邮箱',
        prop:'userEmail'
      },
      {
        label:'用户角色',
        prop:'role',
        formatter(row,column,value){
          return {
            0:"管理员",
            1:"普通用户"
          }[value]
        }
      },
      {
        label:'用户状态',
        prop:'state',
        formatter(row,column,value){
          return {
            1:"在职",
            2:"离职",
            3:"试用期"
          }[value]
        }
      },
      {
        label:'注册时间',
        prop:'createTime'
      },
      {
        label:'最后登陆时间',
        prop:'lastLoginTime'
      }
    ])
```
## 写在最后
文章首发于掘金，后来整理到博客上的，掘金评论区许多大佬都给予了指点，所以有兴趣的可以点击链接过去瞅一瞅大佬们的发言呀~

掘金：[Ned - - 掘金 ](https://juejin.cn/user/105972016875911)

