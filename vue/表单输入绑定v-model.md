# 表单输入绑定v-model

[TOC]

## v-model指令

用在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建**双向数据绑定**，监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理

- `v-model` 在内部为**不同的输入元素**使用**不同的 property** 并抛出**不同事件**：

  - text 和 textarea 元素使用 `value` property 和 `input` 事件
  - checkbox 和 radio 使用 `checked` property 和 `change` 事件
  - select 字段将 `value` 作为 prop 并将 `change` 作为事件

  **注意：**`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值，而总是将 Vue 实例的数据作为数据来源，所以应**在实例数据 data 中设置初始值**

  ```html
  <span>Multiline message is:</span>
  <p style="white-space: pre-line;">{{ message }}</p>
  <br>
  <textarea v-model="message" placeholder="add multiple lines"></textarea>
  <!-- 注意：在文本区域插值<textarea>{{text}}</textarea>并不会生效，应用v-model来代替 -->
  ```

  

## 修饰符

- `.lazy`

  ```html
  <!-- 在“change”时而非“input”时更新 -->
  <input v-model.lazy="msg">
  ```

- `.number`

  将用户的输入值转为数值类型，如果这个值无法被 `parseFloat()` 解析，则会返回原始的值

- `.trim`

  自动过滤用户输入的首尾空白字符

