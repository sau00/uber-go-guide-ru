# Не используйте имена встроенных идентификаторов

[Спецификация языка] Go описывает несколько встроенных
[предобъявленных идентификаторов], которые не следует использовать как имена
в программах на Go.

В зависимости от контекста повторное использование этих идентификаторов
в качестве имён либо затенит оригинал в текущей лексической области видимости
(и во всех вложенных), либо сделает код запутанным. В лучшем случае
компилятор пожалуется; в худшем — такой код может содержать скрытые ошибки,
которые сложно найти поиском.

  [Спецификация языка]: https://go.dev/ref/spec
  [предобъявленных идентификаторов]: https://go.dev/ref/spec#Predeclared_identifiers

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
var error string
// `error` затеняет встроенный тип

// или

func handleErrorMessage(error string) {
    // `error` затеняет встроенный тип
}
```

</td><td>

```go
var errorMessage string
// `error` ссылается на встроенный тип

// или

func handleErrorMessage(msg string) {
    // `error` ссылается на встроенный тип
}
```

</td></tr>
<tr><td>

```go
type Foo struct {
    // Формально эти поля не затеняют
    // встроенные типы, но теперь поиск
    // строк `error` и `string`
    // даёт неоднозначные результаты.
    error  error
    string string
}

func (f Foo) Error() error {
    // `error` и `f.error`
    // визуально похожи
    return f.error
}

func (f Foo) String() string {
    // `string` и `f.string`
    // визуально похожи
    return f.string
}
```

</td><td>

```go
type Foo struct {
    // Поиск строк `error` и `string`
    // теперь однозначен.
    err error
    str string
}

func (f Foo) Error() error {
    return f.err
}

func (f Foo) String() string {
    return f.str
}
```

</td></tr>
</tbody></table>

Обратите внимание: компилятор не выдаёт ошибок при использовании
предобъявленных идентификаторов, но такие инструменты, как `go vet`,
должны корректно указывать на эти и другие случаи затенения.
