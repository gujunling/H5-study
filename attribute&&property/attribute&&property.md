### 什么是 attribute

HTML 标签的预定义和自定义属性我们统称为 attribute

### 什么是 property

JS 原生对象的直接属性我们统称为 property

### 什么是布尔值属性

property 所对应的属性值为布尔值类型的，我们称之为布尔值属性

### 什么是非布尔值属性

property 所对应的属性值为非布尔值类型的，我们称之为非布尔值属性

### attribute 与 property 的同步关系

     非布尔值属性 不论什么情况，property和attribute都会同步
     布尔值属性
              property永远都不会同步attribute
             在没有修改过 property的情况下， attribute会同步property
             在修改过 property的情况下， attribute不会同步property

### 浏览器只认 property，即只有 property 的值才会被浏览器显示

### 用户操作的是 property

### 总结

- 布尔值属性最好使用 prop 方法
- 非布尔值属性最好使用 attr 方法
