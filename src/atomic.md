# Используйте go.uber.org/atomic

Атомарные операции пакета [sync/atomic] работают с «сырыми» типами
(`int32`, `int64` и т. д.), поэтому легко забыть использовать атомарную
операцию для чтения или изменения переменной.

[go.uber.org/atomic] добавляет этим операциям типобезопасность, скрывая
лежащий в основе тип. Кроме того, в нём есть удобный тип `atomic.Bool`.

  [go.uber.org/atomic]: https://pkg.go.dev/go.uber.org/atomic
  [sync/atomic]: https://pkg.go.dev/sync/atomic

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type foo struct {
  running int32  // атомарная
}

func (f *foo) start() {
  if atomic.SwapInt32(&f.running, 1) == 1 {
     // уже запущен…
     return
  }
  // запускаем Foo
}

func (f *foo) isRunning() bool {
  return f.running == 1  // гонка!
}
```

</td><td>

```go
type foo struct {
  running atomic.Bool
}

func (f *foo) start() {
  if f.running.Swap(true) {
     // уже запущен…
     return
  }
  // запускаем Foo
}

func (f *foo) isRunning() bool {
  return f.running.Load()
}
```

</td></tr>
</tbody></table>

> **Примечание переводчика.** Начиная с Go 1.19 в стандартном пакете
> `sync/atomic` тоже есть типизированные обёртки — [`atomic.Bool`],
> [`atomic.Int32`], [`atomic.Int64`], [`atomic.Pointer[T]`] и другие.
> Пример справа компилируется и с ними без изменений, так что в новом коде
> можно обойтись без внешней зависимости.

  [`atomic.Bool`]: https://pkg.go.dev/sync/atomic#Bool
  [`atomic.Int32`]: https://pkg.go.dev/sync/atomic#Int32
  [`atomic.Int64`]: https://pkg.go.dev/sync/atomic#Int64
  [`atomic.Pointer[T]`]: https://pkg.go.dev/sync/atomic#Pointer
