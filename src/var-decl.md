# Объявление локальных переменных

Если переменной явно присваивается значение, используйте короткое объявление
переменной (`:=`).

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
var s = "foo"
```

</td><td>

```go
s := "foo"
```

</td></tr>
</tbody></table>

Однако бывают случаи, когда значение по умолчанию понятнее при использовании
ключевого слова `var`. Например, при [объявлении пустых срезов].

  [объявлении пустых срезов]: https://go.dev/wiki/CodeReviewComments#declaring-empty-slices

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func f(list []int) {
  filtered := []int{}
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td><td>

```go
func f(list []int) {
  var filtered []int
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td></tr>
</tbody></table>
