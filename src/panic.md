# Не паникуйте

Код, работающий в продакшене, должен избегать паник. Паники — основной
источник [каскадных сбоев]. Если возникла ошибка, функция должна вернуть её
и позволить вызывающему коду решить, как её обработать.

  [каскадных сбоев]: https://en.wikipedia.org/wiki/Cascading_failure

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func run(args []string) {
  if len(args) == 0 {
    panic("an argument is required")
  }
  // ...
}

func main() {
  run(os.Args[1:])
}
```

</td><td>

```go
func run(args []string) error {
  if len(args) == 0 {
    return errors.New("an argument is required")
  }
  // ...
  return nil
}

func main() {
  if err := run(os.Args[1:]); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

</td></tr>
</tbody></table>

Panic/recover — это не стратегия обработки ошибок. Программа должна паниковать
только тогда, когда произошло что-то непоправимое, например разыменование nil.
Исключение — инициализация программы: если при запуске случилось что-то плохое,
из-за чего программу следует прервать, допустима паника.

```go
var _statusTemplate = template.Must(template.New("name").Parse("_statusHTML"))
```

Даже в тестах вместо паники предпочитайте `t.Fatal` или `t.FailNow`, чтобы
тест гарантированно был помечен как проваленный.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  panic("failed to set up test")
}
```

</td><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  t.Fatal("failed to set up test")
}
```

</td></tr>
</tbody></table>
