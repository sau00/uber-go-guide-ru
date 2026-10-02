# Объявление переменных верхнего уровня

На верхнем уровне используйте стандартное ключевое слово `var`. Не указывайте
тип, если он совпадает с типом выражения.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
var _s string = F()

func F() string { return "A" }
```

</td><td>

```go
var _s = F()
// F уже объявляет, что возвращает string,
// поэтому повторно указывать тип не нужно.

func F() string { return "A" }
```

</td></tr>
</tbody></table>

Указывайте тип, если тип выражения не совпадает с нужным типом в точности.

```go
type myError struct{}

func (myError) Error() string { return "error" }

func F() myError { return myError{} }

var _e error = F()
// F возвращает объект типа myError, а нам нужен error.
```
