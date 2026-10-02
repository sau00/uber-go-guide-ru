# Группировка и порядок функций

- Функции должны быть упорядочены примерно в порядке их вызова.
- Функции в файле должны быть сгруппированы по получателю.

Следовательно, экспортируемые функции должны идти в файле первыми, после
определений `struct`, `const` и `var`.

Функция `newXYZ()`/`NewXYZ()` может идти сразу после определения типа,
но перед остальными методами этого получателя.

Поскольку функции сгруппированы по получателю, простые вспомогательные функции
должны располагаться ближе к концу файла.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func (s *something) Cost() {
  return calcCost(s.weights)
}

type something struct{ ... }

func calcCost(n []int) int {...}

func (s *something) Stop() {...}

func newSomething() *something {
    return &something{}
}
```

</td><td>

```go
type something struct{ ... }

func newSomething() *something {
    return &something{}
}

func (s *something) Cost() {
  return calcCost(s.weights)
}

func (s *something) Stop() {...}

func calcCost(n []int) int {...}
```

</td></tr>
</tbody></table>
