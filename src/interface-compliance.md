# Проверяйте соответствие интерфейсу

Там, где это уместно, проверяйте соответствие интерфейсу на этапе компиляции.
Это касается:

- экспортируемых типов, которые по контракту своего API обязаны реализовывать
  определённые интерфейсы;
- экспортируемых и неэкспортируемых типов, входящих в набор типов,
  реализующих один и тот же интерфейс;
- других случаев, когда несоответствие интерфейсу сломает код пользователей.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type Handler struct {
  // ...
}



func (h *Handler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  ...
}
```

</td><td>

```go
type Handler struct {
  // ...
}

var _ http.Handler = (*Handler)(nil)

func (h *Handler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```

</td></tr>
</tbody></table>

Выражение `var _ http.Handler = (*Handler)(nil)` перестанет компилироваться,
если `*Handler` когда-нибудь перестанет соответствовать интерфейсу `http.Handler`.

В правой части присваивания должно стоять нулевое значение проверяемого типа:
`nil` для указателей (например, `*Handler`), срезов и мап и пустая структура
для структурных типов.

```go
type LogHandler struct {
  h   http.Handler
  log *zap.Logger
}

var _ http.Handler = LogHandler{}

func (h LogHandler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```
