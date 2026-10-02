# Нулевое значение мьютекса валидно

Нулевое значение `sync.Mutex` и `sync.RWMutex` валидно, поэтому указатель
на мьютекс почти никогда не нужен.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
mu := new(sync.Mutex)
mu.Lock()
```

</td><td>

```go
var mu sync.Mutex
mu.Lock()
```

</td></tr>
</tbody></table>

Если вы работаете со структурой через указатель, мьютекс должен быть
её полем-значением (не указателем). Не встраивайте мьютекс в структуру,
даже если структура не экспортируется.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type SMap struct {
  sync.Mutex

  data map[string]string
}

func NewSMap() *SMap {
  return &SMap{
    data: make(map[string]string),
  }
}

func (m *SMap) Get(k string) string {
  m.Lock()
  defer m.Unlock()

  return m.data[k]
}
```

</td><td>

```go
type SMap struct {
  mu sync.Mutex

  data map[string]string
}

func NewSMap() *SMap {
  return &SMap{
    data: make(map[string]string),
  }
}

func (m *SMap) Get(k string) string {
  m.mu.Lock()
  defer m.mu.Unlock()

  return m.data[k]
}
```

</td></tr>

<tr><td>

Поле `Mutex` и методы `Lock` и `Unlock` непреднамеренно становятся частью
экспортируемого API `SMap`.

</td><td>

Мьютекс и его методы — детали реализации `SMap`, скрытые от вызывающего кода.

</td></tr>
</tbody></table>
