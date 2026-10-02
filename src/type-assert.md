# Обрабатывайте неудачные приведения типов

[Приведение типа] (type assertion) в форме с одним возвращаемым значением
вызывает панику, если тип неверный. Поэтому всегда используйте идиому «comma ok».

  [Приведение типа]: https://go.dev/ref/spec#Type_assertions

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
t := i.(string)
```

</td><td>

```go
t, ok := i.(string)
if !ok {
  // аккуратно обрабатываем ошибку
}
```

</td></tr>
</tbody></table>

<!-- TODO: There are a few situations where the single assignment form is
fine. -->
