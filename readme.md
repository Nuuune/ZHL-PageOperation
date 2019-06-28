# 关于PageOperation的使用说明

请在此项目npm start后访问基础使用示例页  
访问基础使用示例页的路由为 *http://localhost:8080/#/pagetemplate*  
示例页的源码在 *src/page-example/index.vue*
---

## 一、组件导入
```html
<template>
  ...
</template>
<script>
/* 方法一 */
import PageOperation from '@/components/PageOperation'

export default {
  components: {
    PageOperation
  },
  ...
}

/* 方法二 */
import { page } from '@/mixins/page'

export default {
  mixins: [page],
  ...
}

</script>
```

*上述的两种方式的导入后即可在template部分开始使用此组件了*

## 二、组件接受的参数
### 1.数据
* [searchBool](#searchBool)
* [searchObj](#searchObj)
* [table](#table)
* [pagination](#pagination)
* [dialog](#dialog)
* [getDataList](#getDataList)
* [loadBool](#loadBool)
* [value](#value)
* [pageId](#pageId)
### 2.事件
* [submit](#submit)
* [detail](#detail)
* [add](#add)
* [edit](#edit)
* [del](#del)
### 3.其他说明
* [bus的使用](#bus-uses)
---

>#### 数据部分

<span id="searchBool"></span>
**searchBool**  

类型|默认值|说明  
:--:|:--:|:--  
布尔值|false|false: 搜索模块的表单控件会显示，即**searchObj**所定义会展示<br>true:搜索模块的表单控件会隐藏  

---
<span id="searchObj"></span>
**searchObj**  

类型|默认值|说明
:--:|:--:|:--
数组|[]|搜索模块会遍历此数组来生成对应的表单控件  

*此数组元素(类型为对象)说明*  

字段|值类型|说明
:--:|:--|:--
isXXX|布尔值(true/false)|XXX的值有如下:<br>1.Number: isNumber生成一个输入框并输入类型限定为数字<br>2.Text: isText生成一个普通文本输入框<br>3.Select: isSelect生成一个下拉选择框，下拉框的选项由此元素的list字段提供<br>4.DateRange: isDateRange生成一个日期范围选择器<br>5.Date: isDate生成一个日期选择器
value|字符串|在此searchObj数组里每个元素都需要不同value值, 可以理解为需要做列表请求的参数的key值
label|与对应控件的值的类型对应|该字段将与控件进行双向绑定，即该值会影响控件，控件也能改变该值
name|字符串|用于给这些输入控件加一个说明前缀---日期类型控件无效
list|数组|只当isSelect的清况下有效，为下拉框提供选项, 此数组元素示例{label:'展现的文字', value: '真实的值'}

---
<span id="table"></span>
**table**  

table为对象类型

字段|值类型|说明
:--:|:--|:--
searchBool|布尔值-默认为false|控制搜索模块中的搜索按钮的显示与否, false-显示
addBool|布尔值-默认为false|控制搜索模块中的新增按钮的显示与否, false-显示
emitAddBool|布尔值-默认为false|控制当新增按钮按下时，是由开发者自行处理新增事件 ***@add*** 还是由组件自行处理, 默认为false<br> true: 开发者处理; false: 组件处理
tableBool|布尔值-默认为false|控制整个table表格的显示与否, false-显示
tablePagBool|布尔值-默认为false|控制table下方的分页器的显示与否, false-显示
getTableList|布尔值-默认为false|控制table表格里数据是否开始加载, false-可以加载
columns|数组-此字段必填|用于定义表格数据的列展示,元素为对象---{<br>value: '将要展示的数据的字段名', <br>label: '此列的表头名', <br>align: '此列文本的对齐方式', <br>width: '此列的宽度', <br>slotBool: '默认false; true表示此列的展示方式由自定义插槽来完成', <br>formatter: 'function(row, colName, value, index) 此方法的返回值会替换原本的参数value'<br>}<br>除了value、label外，其它都为可选属性
width|数值或字符串-默认为150|操作按钮列的宽度定义
btn|布尔值-默认为false|控制操作按钮列的显示与否, false-显示
detailBool|布尔值-默认为false|控制操作按钮列中的查看按钮显示与否, false-不显示
editBool|布尔值-默认为false|控制操作按钮列中的编辑按钮显示与否, false-显示
delBool|布尔值-默认为false|控制操作按钮列中的删除按钮显示与否, false-显示
emitDetailBool|布尔值-默认为false|控制当查看按钮按下时, 是否触发此组件的 ***@detail*** 事件; true-触发
emitEditBool|布尔值-默认为false|控制当编辑按钮按下时, 是否触发此组件的 ***@edit*** 事件; true-触发

---
<span id="pagination"></span>
**pagination**  

table下方的分页器配置  
类型为对象类型  

字段|值类型|说明
:--:|:--|:--
page|数值|当前的页码数
count|数值|当前页的显示的数据条数
total|数值|全部的数据条数

---
<span id="dialog"></span>
**dialog**  

表单弹窗的配置  
类型为对象  

字段|值类型|说明
:--:|:--|:--
title|字符串-默认路由的title|弹窗的标题名
width|字符串-默认'50%'|弹窗的宽度
saveFormVisible|布尔值-默认false|控制弹窗是否展示, false-不展示
fullscreen|布尔值-默认false|控制弹窗是否全屏, false-不全屏
top|字符串-默认'15vh'|弹窗距离顶部的距离， 值为任意css的距离支持值
appendToBody|布尔值-false|弹窗在dom上的渲染是否放置到body标签---如有其他元素层级比较高，遮住此弹窗，那么可以试着将此值置为true
ruleForm2|对象-必传|你要操作的表单数据
rules2|对象|此表单的验证规则 --- 可以参考element表单部分的[rules](https://element.eleme.io/#/zh-CN/component/form#biao-dan-yan-zheng)字段

---
<span id="getDataList"></span>
**getDataList**  

获取表格数据的方法  

```javascript
// 写法一 --- 返回请求方法
getDataList = function(pageNum) {
  
  // 这里可以处理一些请求的其他参数

  return api.someFunciton({
    ...,
    pageNum: pageNum ? pageNum : this.pagination.page,
    ...,
  })
}

// 写法二 --- 返回promise包裹的请求返回值
getDataList = async function(pageNum) {

  // 这里可以处理一些请求的其他参数

  try {
    let res = await api.someFunciton({
      ...,
      pageNum: pageNum ? pageNum : this.pagination.page,
      ...,
    })
    return res
  } catch (e) {
    console.error('请求出错', e)
    return Promise.reject(e)
  }
}
```

---
<span id="loadBool"></span><span id="value"></span>
**loadBool**与**value**  

loadBool 默认为false，为true时 当value变化时 table的表格数据将会通过getDataList方法向服务器重新获得

---
<span id="pageId"></span>
**pageId**  

pageId数据类型为任意字符串，可以不传  
用途主要是用于在同一路由下使用了两个PageOperation组件情况；用于事件触发只会触发指定的PageOperation组件来响应

---
>#### 事件部分

<span id="submit"></span>
**submit**  

@submit="handleSubmit"  

触发情形|参数说明
:--|:--
当dialog表单弹窗的提交按钮被按下时触发|handleSubmit(ruleForm2, savePrompt)<br>ruleForm2: 表单数据<br>savePrompt: function(apiRequest, apiRequestParams) 一个方法来触发submit请求<br>apiRequest: 你定义的api请求方法<br>apiRequestParams: 你请求所需要的参数

---
<span id="detail"></span>
**detail**  

@detail="handleDetail"  

触发情形|参数说明
:--|:--
table.emitDetailBool为true时,<br>且按下table操作列中的查看按钮触发|handleDetail(ruleForm2)<br>ruleForm2: 此行的数据, 如果要对此进行操作, 请先copy一份来操作, 以免影响table中此行的数据

---
<span id="add"></span>
**add**  

@add="handleAdd"  

触发情形|参数说明
:--|:--
table.emitAddBool为true时,<br>且按下搜索模块中的新增按钮触发|handleAdd(ruleForm2)<br>ruleForm2: 弹窗表单对象, 可以对其作一些初始化操作

---
<span id="edit"></span>
**edit**  

@edit="handleEdit"  

触发情形|参数说明
:--|:--
table.emitEditBool为true时,<br>且按下table操作列中的编辑按钮触发|handleEdit(ruleForm2)<br>ruleForm2: 弹窗表单对象, 可以对其作一些初始化操作

---
<span id="del"></span>
**del**  

@edit="handleDel"  

触发情形|参数说明
:--|:--
table操作列中的删除按钮时触发|handleDel(row, delPrompt)<br>row: 此行的数据<br>delPrompt:function(apiRequest, apiRequestParams) 一个方法来触发del请求<br>apiRequest: 你定义的api请求方法<br>apiRequestParams: 你请求所需要的参数

---
>#### 其它说明

<span id="bus-uses"></span>
**bus的使用** 

***导入bus,从父级刷新指定PageOperation的列表数据***  

```html
<template>
  <div>
    <!-- 第一个PageOperation组件 -->
    <PageOperation
      ...props
      ></PageOperation>
    <!-- 第二个PageOperation组件 -->
    <PageOperation
      ...props
      pageId="page2"
      ></PageOperation>
  </div>
</template>
<script>
import PageOperation from '@/components/PageOperation'
import bus from '@/utils/bus' // 这一步导入

export default {
  components: {
    PageOperation
  },
  ...,
  methods: {
    someFunction1() {
      // do sth ...
      bus.$emit('loadDialogData') // 这一步将会刷新无pageId的PageOperation的table数据
    },
    someFunction2() {
      // do sth ...
      bus.$emit('loadDialogData-page2') // 这一步将会刷新pageId为page2的PageOperation的table数据
    },
  }
}

</script>
```
