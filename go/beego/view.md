# beego view语法

{{.}} 获取当前对象的值



循环

```html
{{range .`对象`}}
{{end}}
```



条件处理

if里面只能是bool值，不能写运算符

```html
{{if `条件`}}...
{{elseif }}...
{{else}}...
{{end}}
```



pipelines

```html
{{ `对象` | `方法` }}
```



