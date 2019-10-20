---
title: Stylus学习
date: 2019-8-28 16:30:05
tags:
---
## Stylus

[stylus参考文档](https://www.zhangxinxu.com/jq/stylus/variables.php)

### 分号，冒号，大括号是可选的

```stylus
body button,
body button.button,
body input[type='button'],
body input[type='submit'] {
  border-radius: 5px;
}
```

### 同样支持父选择器引用

```stylus
ul
  li a
    display: block
    color: blue
    padding: 5px
    html.ie &
      padding: 6px
    &:hover
      color: red
```

### 混入Mixins
```stylus
  border-radius(val)
  -webkit-border-radius: val
  -moz-border-radius: val
  border-radius: val
  
button {
  border-radius  5px; //不是通过括号来调用的
}
```
因此，我们可以利用arguments这个局部变量，传递可以包含多值的表达式。
```stylus
border-radius()
  -webkit-border-radius arguments
  -moz-border-radius arguments
  border-radius arguments
button {
  border-radius  5px 2px; 
}
```
### 方法
**可以返回值**
```stylus
add(a, b)
  a + b
body 
  padding add(10px, 5)
```
编译后
```css
body {
  padding: 15px;
}
```
**默认参数**

```stylus
add(a, b = unit(a, px))
  a + b
```

**多个返回值**

```stylus
sizes()
 15px 10px

sizes()[0] // 15px
```
**变量函数**

```stylus
invoke(a, b, fn)
  fn(a, b)

add(a, b)
  a + b

body
  padding invoke(5, 10, add)
  padding invoke(5, 10, sub)
```

### 变量
**标识符 `$`**


```stylus
	$bgColor = red
	div{
		background:$bgColor
	}
```
**属性查找**

例如： 元素水平垂直居中对齐（使用百分比和margin负值）
```stylus
	#logo
	  position: absolute
	  top: 50%
	  left: 50%
	  width: w = 150px
	  height: h = 80px
	  margin-left: -(w / 2)
	  margin-top: -(h / 2)
```
可以改写为
```stylus
	#logo
	  position: absolute
	  top: 50%
	  left: 50%
	  width: 150px
	  height: 80px
	  margin-left: -(@width / 2)
	  margin-top: -(@height / 2)
```

### 消除歧义

避免被解释成减法运算

```stylus
pad(n)
	padding (-n) 
body
	pad(5px)
```
### 插值

**私有前缀属性扩展**

使用`{}`来插入值
```stylus
vendor(prop, args)
  -webkit-{prop} args
  -moz-{prop} args
  {prop} args

border-radius()
  vendor('border-radius', arguments)

box-shadow()
  vendor('box-shadow', arguments)

button
  border-radius 1px 2px / 3px 4px
```
编译后
```stylus
button {
  -webkit-border-radius: 1px 2px / 3px 4px;
  -moz-border-radius: 1px 2px / 3px 4px;
  border-radius: 1px 2px / 3px 4px;
}
```

**选择器插值**

```javascript
table
  for row in 1 2 3 4 5
    tr:nth-child({row})
      height: 10px * row
```
编译后
```stylus
table tr:nth-child(1) {
  height: 10px;
}
table tr:nth-child(2) {
  height: 20px;
}
table tr:nth-child(3) {
  height: 30px;
}
table tr:nth-child(4) {
  height: 40px;
}
table tr:nth-child(5) {
  height: 50px;
}
```

### 运算符
下列运算符优先级从高到低
```
[]
! ~ + -
is defined
** * / %
+ -
... ..
<= >= < >
in
== is != is not isnt
is a
&& and || or
?:
= := ?= += -= *= /= %=
not
if unless
```
这里只列举陌生的运算符

**逻辑运算符**

- `&&`同 `and`
- `||` 同 `or`
- `not`
```stylus
not true // false
not not true //true
```

**下标运算符**

**范围**

```stylus
1..5 // 1 2 3 4 5
1...5 // 1 2 3 4
```
**加减**
```stylus
15px - 5px
// => 10px

5 - 2
// => 3

5in - 50mm
// => 3.031in

5s - 1000ms
// => 4s

20mm + 4in
// => 121.6mm

"foo " + "bar"
// => "foo bar"
``` 

**乘除**

当在属性值内使用/时候，你必须用括号包住

```stylus
font: (14px/1.5);
```
**指数**
```stylus
2 ** 8 //256
```
**存在操作符in**

```stylus
1 in 1..3  // true
```

### 内置方法
[内置方法](https://www.zhangxinxu.com/jq/stylus/bifs.php)
- `unit()`
```stylus
unit(10)
// => ''

unit(15in)
// => 'in'

unit(15%, 'px')
// => 15px

unit(15%, px)
// => 15px
```
