# Используйте имена полей при инициализации структур

При инициализации структур почти всегда следует указывать имена полей.
Теперь этого требует [`go vet`].

  [`go vet`]: https://pkg.go.dev/cmd/vet

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
k := User{"John", "Doe", true}
```

</td><td>

```go
k := User{
    FirstName: "John",
    LastName: "Doe",
    Admin: true,
}
```

</td></tr>
</tbody></table>

Исключение: в тестовых таблицах имена полей *можно* опускать,
если полей 3 или меньше.

```go
tests := []struct{
  op Operation
  want string
}{
  {Add, "add"},
  {Subtract, "subtract"},
}
```
