# Типы ошибок

Есть несколько способов объявить ошибку.
Прежде чем выбрать наиболее подходящий для вашего случая, ответьте на вопросы:

- Нужно ли вызывающему коду распознавать ошибку, чтобы обработать её?
  Если да, нужно поддержать функции [`errors.Is`] или [`errors.As`],
  объявив переменную ошибки верхнего уровня или собственный тип.
- Сообщение об ошибке — статическая строка
  или динамическая, требующая контекстной информации?
  В первом случае можно использовать [`errors.New`], во втором —
  [`fmt.Errorf`] или собственный тип ошибки.
- Передаём ли мы дальше новую ошибку, полученную от нижележащей функции?
  Если да, смотрите [раздел об оборачивании ошибок](error-wrap.md).

[`errors.Is`]: https://pkg.go.dev/errors#Is
[`errors.As`]: https://pkg.go.dev/errors#As

| Распознавание ошибки? | Сообщение    | Рекомендация                                  |
|-----------------------|--------------|-----------------------------------------------|
| Нет                   | статическое  | [`errors.New`]                                |
| Нет                   | динамическое | [`fmt.Errorf`]                                |
| Да                    | статическое  | `var` верхнего уровня с [`errors.New`]        |
| Да                    | динамическое | собственный тип `error`                       |

[`errors.New`]: https://pkg.go.dev/errors#New
[`fmt.Errorf`]: https://pkg.go.dev/fmt#Errorf

Например, для ошибки со статической строкой используйте [`errors.New`].
Если вызывающему коду нужно распознавать и обрабатывать эту ошибку,
экспортируйте её как переменную, чтобы её можно было проверить с помощью `errors.Is`.

<table>
<thead><tr><th>Без распознавания ошибки</th><th>С распознаванием ошибки</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open() error {
  return errors.New("could not open")
}

// package bar

if err := foo.Open(); err != nil {
  // Обработать ошибку невозможно.
  panic("unknown error")
}
```

</td><td>

```go
// package foo

var ErrCouldNotOpen = errors.New("could not open")

func Open() error {
  return ErrCouldNotOpen
}

// package bar

if err := foo.Open(); err != nil {
  if errors.Is(err, foo.ErrCouldNotOpen) {
    // обрабатываем ошибку
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Для ошибки с динамической строкой используйте [`fmt.Errorf`], если вызывающему
коду не нужно её распознавать, и собственный тип `error`, если нужно.

<table>
<thead><tr><th>Без распознавания ошибки</th><th>С распознаванием ошибки</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open(file string) error {
  return fmt.Errorf("file %q not found", file)
}

// package bar

if err := foo.Open("testfile.txt"); err != nil {
  // Обработать ошибку невозможно.
  panic("unknown error")
}
```

</td><td>

```go
// package foo

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

func Open(file string) error {
  return &NotFoundError{File: file}
}


// package bar

if err := foo.Open("testfile.txt"); err != nil {
  var notFound *NotFoundError
  if errors.As(err, &notFound) {
    // обрабатываем ошибку
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Учтите, что экспортируемые из пакета переменные и типы ошибок
становятся частью его публичного API.
