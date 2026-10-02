# Строки формата вне Printf

Если вы объявляете строки формата для функций в стиле `Printf` не в виде
строкового литерала прямо в вызове, делайте их константами (`const`).

Это помогает `go vet` выполнять статический анализ строки формата.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
msg := "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td><td>

```go
const msg = "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td></tr>
</tbody></table>
