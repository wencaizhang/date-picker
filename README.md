# 基于 vant 的日期选择组件

基于 vant 的 popup 和 picker 组件开发的一个日期选择组件。

## 描述

指定开始时间（`startTime`）和时间选择范围（`during`），以 15 分钟（`interval`）为间隔，选择某一个时间点。

## 预览

预览地址（如果是 PC 端请使用开发者工具将浏览器调成手机模式）：[https://wencaizhang.github.io/date-picker/](https://wencaizhang.github.io/date-picker/)

或者直接使用手机扫描二维码：

![扫描二维码预览](./images/QR.png)

## 使用

拷贝位于 [`src/components`](./src/components/) 目录下的文件至你自己的项目中即可，其中 `picker.vue` 处理主要逻辑，`util.js` 主要是时间转换的工具函数。

需要注意本组件依赖于 [vant 组件库](https://youzan.github.io/vant/#/zh-CN/intro)的 popup 和 picker 组件。

## 可配置项

| 字段      | 说明                                  |      类型 |  默认值  |
| --------- | ------------------------------------- | -------- | ------ |
| `visible`   | 当前组件是否显示                      | `Boolean` | `false`  |
| `startTime` | 开始时间, 格式: `2019-05-26 11:07:57` |  `String` | 当前时间 |
| `during`    | 时间选择范围（单位:天）               |  `Number` |   `90`   |
| `interval`  | 时间选择间隔（单位:分钟）             |  `Number` |   `15`   |
| `overlay`   | 是否显示遮罩层                        | `Boolean` |  `true`  |
| `position`  | 位置，可选值为 top bottom right left  |  `String` | `bottom` |
