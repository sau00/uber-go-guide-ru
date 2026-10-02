# Функциональные опции

Функциональные опции (functional options) — это паттерн, при котором вы
объявляете непрозрачный тип `Option`, записывающий информацию в некоторую
внутреннюю структуру. Функция принимает произвольное число таких опций
и действует на основе всей информации, записанной опциями во внутреннюю
структуру.

Используйте этот паттерн для необязательных аргументов конструкторов и других
публичных API, которые, как вы предвидите, придётся расширять, — особенно если
у этих функций уже три аргумента или больше.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// package db

func Open(
  addr string,
  cache bool,
  logger *zap.Logger,
) (*Connection, error) {
  // ...
}
```

</td><td>

```go
// package db

type Option interface {
  // ...
}

func WithCache(c bool) Option {
  // ...
}

func WithLogger(log *zap.Logger) Option {
  // ...
}

// Open создаёт соединение.
func Open(
  addr string,
  opts ...Option,
) (*Connection, error) {
  // ...
}
```

</td></tr>
<tr><td>

Параметры cache и logger нужно передавать всегда, даже если пользователь
хочет использовать значения по умолчанию.

```go
db.Open(addr, db.DefaultCache, zap.NewNop())
db.Open(addr, db.DefaultCache, log)
db.Open(addr, false /* cache */, zap.NewNop())
db.Open(addr, false /* cache */, log)
```

</td><td>

Опции передаются только при необходимости.

```go
db.Open(addr)
db.Open(addr, db.WithLogger(log))
db.Open(addr, db.WithCache(false))
db.Open(
  addr,
  db.WithCache(false),
  db.WithLogger(log),
)
```

</td></tr>
</tbody></table>

Мы предлагаем реализовывать этот паттерн через интерфейс `Option`
с неэкспортируемым методом, который записывает опции в неэкспортируемую
структуру `options`.

```go
type options struct {
  cache  bool
  logger *zap.Logger
}

type Option interface {
  apply(*options)
}

type cacheOption bool

func (c cacheOption) apply(opts *options) {
  opts.cache = bool(c)
}

func WithCache(c bool) Option {
  return cacheOption(c)
}

type loggerOption struct {
  Log *zap.Logger
}

func (l loggerOption) apply(opts *options) {
  opts.logger = l.Log
}

func WithLogger(log *zap.Logger) Option {
  return loggerOption{Log: log}
}

// Open создаёт соединение.
func Open(
  addr string,
  opts ...Option,
) (*Connection, error) {
  options := options{
    cache:  defaultCache,
    logger: zap.NewNop(),
  }

  for _, o := range opts {
    o.apply(&options)
  }

  // ...
}
```

Этот паттерн можно реализовать и с помощью замыканий, но мы считаем, что
приведённый выше вариант даёт авторам больше гибкости, а пользователям его
проще отлаживать и тестировать. В частности, он позволяет сравнивать опции
между собой в тестах и моках, что с замыканиями невозможно. Кроме того,
опции могут реализовывать другие интерфейсы, включая `fmt.Stringer`, что
позволяет получать понятные человеку строковые представления опций.

См. также:

- [Self-referential functions and the design of options]
- [Functional options for friendly APIs]

  [Self-referential functions and the design of options]: https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html
  [Functional options for friendly APIs]: https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->
