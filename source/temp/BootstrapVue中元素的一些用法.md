### 表单
表单的一项经常是一些文字带一个表单input，比如标题，要写清楚标题两个字，旁边附带一个输入框。
则就是bootstrap中表单组的概念。
```html
<b-form-group label="性别">
    <b-form-radio-group v-model="form.gender" :disabled="disabled">
        <b-form-radio value="true">男性</b-form-radio>
        <b-form-radio value="false">女性</b-form-radio>
    </b-form-radio-group>
</b-form-group>
```