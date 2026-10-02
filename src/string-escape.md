# Используйте сырые строковые литералы, чтобы избежать экранирования

Go поддерживает [сырые строковые литералы](https://go.dev/ref/spec#raw_string_lit)
(raw string literals), которые могут занимать несколько строк и содержать кавычки.
Используйте их, чтобы избежать строк с ручным экранированием, которые гораздо
труднее читать.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
wantError := "unknown name:\"test\""
```

</td><td>

```go
wantError := `unknown name:"test"`
```

</td></tr>
</tbody></table>
