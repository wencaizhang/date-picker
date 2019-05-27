# 基于 vant 的日期选择组件

基于 vant 的 popup 和 picker 组件开发

## 描述

指定开始时间（startTime）和时间选择范围（during），以 15 分钟(space)为间隔，选择某一个时间点。

## 可配置项

| 字段      | 说明                                  |      类型 |  默认值  |
|-----------|---------------------------------------|----------:|:--------:|
| visible   | 当前组件是否显示                      | `Boolean` | `false`  |
| startTime | 开始时间, 格式: `2019-05-26 11:07:57` |  `String` | 当前时间 |
| during    | 时间选择范围(单位:天)                 |  `Number` |   `90`   |
| space     | 时间选择间隔(单位:分钟)                 |  `Number` |   `15`   |
| overlay   | 是否显示遮罩层                        | `Boolean` |  `true`  |
| position  | 位置，可选值为 top bottom right left  |  `String` | `bottom` |