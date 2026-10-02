# Инициализация ссылок на структуры

При инициализации ссылок на структуры используйте `&T{}` вместо `new(T)`,
чтобы это было единообразно с инициализацией структур.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
sval := T{Name: "foo"}

// неединообразно
sptr := new(T)
sptr.Name = "bar"
```

</td><td>

```go
sval := T{Name: "foo"}

sptr := &T{Name: "bar"}
```

</td></tr>
</tbody></table>
