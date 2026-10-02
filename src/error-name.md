# Именование ошибок

Для значений ошибок, хранящихся в глобальных переменных, используйте префикс
`Err` или `err` в зависимости от того, экспортируются ли они.
Эта рекомендация имеет приоритет над правилом
[Используйте префикс _ для неэкспортируемых глобальных переменных](global-name.md).

```go
var (
  // Следующие две ошибки экспортируются,
  // чтобы пользователи пакета могли распознавать их
  // с помощью errors.Is.

  ErrBrokenLink = errors.New("link is broken")
  ErrCouldNotOpen = errors.New("could not open")

  // Эта ошибка не экспортируется, потому что
  // мы не хотим делать её частью публичного API.
  // Внутри пакета её всё равно можно использовать
  // с errors.Is.

  errNotFound = errors.New("not found")
)
```

Для собственных типов ошибок используйте суффикс `Error`.

```go
// Эта ошибка тоже экспортируется,
// чтобы пользователи пакета могли распознавать её
// с помощью errors.As.

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

// А эта ошибка не экспортируется, потому что
// мы не хотим делать её частью публичного API.
// Внутри пакета её всё равно можно использовать
// с errors.As.

type resolveError struct {
  Path string
}

func (e *resolveError) Error() string {
  return fmt.Sprintf("resolve %q", e.Path)
}
```
