# Используйте пакет `"time"` для работы со временем

Время — сложная штука. Вот некоторые неверные предположения о времени,
которые часто делают:

1. В сутках 24 часа
2. В часе 60 минут
3. В неделе 7 дней
4. В году 365 дней
5. [И многое другое](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)

Например, из пункта *1* следует, что прибавление 24 часов к моменту времени
не всегда даёт новый календарный день.

Поэтому для работы со временем всегда используйте пакет [`"time"`]: он помогает
справляться с этими неверными предположениями безопаснее и точнее.

  [`"time"`]: https://pkg.go.dev/time

## Используйте `time.Time` для моментов времени

Используйте [`time.Time`] для работы с моментами времени, а для сравнения,
сложения и вычитания времени — методы `time.Time`.

  [`time.Time`]: https://pkg.go.dev/time#Time

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func isActive(now, start, stop int) bool {
  return start <= now && now < stop
}
```

</td><td>

```go
func isActive(now, start, stop time.Time) bool {
  return (start.Before(now) || start.Equal(now)) && now.Before(stop)
}
```

</td></tr>
</tbody></table>

## Используйте `time.Duration` для промежутков времени

Используйте [`time.Duration`] для работы с промежутками времени.

  [`time.Duration`]: https://pkg.go.dev/time#Duration

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func poll(delay int) {
  for {
    // ...
    time.Sleep(time.Duration(delay) * time.Millisecond)
  }
}

poll(10) // это секунды или миллисекунды?
```

</td><td>

```go
func poll(delay time.Duration) {
  for {
    // ...
    time.Sleep(delay)
  }
}

poll(10*time.Second)
```

</td></tr>
</tbody></table>

Вернёмся к примеру с прибавлением 24 часов к моменту времени: метод,
которым мы прибавляем время, зависит от намерения. Если нужно то же время суток,
но на следующий календарный день, следует использовать [`Time.AddDate`].
Если же нужен момент времени, гарантированно отстоящий от предыдущего
ровно на 24 часа, следует использовать [`Time.Add`].

  [`Time.AddDate`]: https://pkg.go.dev/time#Time.AddDate
  [`Time.Add`]: https://pkg.go.dev/time#Time.Add

```go
newDay := t.AddDate(0 /* годы */, 0 /* месяцы */, 1 /* дни */)
maybeNewDay := t.Add(24 * time.Hour)
```

## Используйте `time.Time` и `time.Duration` при взаимодействии с внешними системами

По возможности используйте `time.Duration` и `time.Time` при взаимодействии
с внешними системами. Например:

- Флаги командной строки: [`flag`] поддерживает `time.Duration` с помощью
  [`time.ParseDuration`]
- JSON: [`encoding/json`] поддерживает кодирование `time.Time` в строку
  формата [RFC 3339] благодаря [методу `UnmarshalJSON`]
- SQL: [`database/sql`] поддерживает преобразование столбцов `DATETIME`
  и `TIMESTAMP` в `time.Time` и обратно, если это поддерживает драйвер
- YAML: [`gopkg.in/yaml.v2`] поддерживает `time.Time` в виде строки формата
  [RFC 3339] и `time.Duration` с помощью [`time.ParseDuration`].

  [`flag`]: https://pkg.go.dev/flag
  [`time.ParseDuration`]: https://pkg.go.dev/time#ParseDuration
  [`encoding/json`]: https://pkg.go.dev/encoding/json
  [RFC 3339]: https://tools.ietf.org/html/rfc3339
  [методу `UnmarshalJSON`]: https://pkg.go.dev/time#Time.UnmarshalJSON
  [`database/sql`]: https://pkg.go.dev/database/sql
  [`gopkg.in/yaml.v2`]: https://pkg.go.dev/gopkg.in/yaml.v2

Если использовать `time.Duration` при таком взаимодействии невозможно,
используйте `int` или `float64` и указывайте единицу измерения в имени поля.

Например, поскольку `encoding/json` не поддерживает `time.Duration`,
единица измерения включена в имя поля.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// {"interval": 2}
type Config struct {
  Interval int `json:"interval"`
}
```

</td><td>

```go
// {"intervalMillis": 2000}
type Config struct {
  IntervalMillis int `json:"intervalMillis"`
}
```

</td></tr>
</tbody></table>

Если использовать `time.Time` при таком взаимодействии невозможно, то, если
не договорились об ином, используйте `string` и форматируйте метки времени
согласно [RFC 3339]. Этот формат по умолчанию используется в
[`Time.UnmarshalText`] и доступен для `Time.Format` и `time.Parse`
через константу [`time.RFC3339`].

  [`Time.UnmarshalText`]: https://pkg.go.dev/time#Time.UnmarshalText
  [`time.RFC3339`]: https://pkg.go.dev/time#RFC3339

Хотя на практике это обычно не вызывает проблем, имейте в виду, что пакет
`"time"` не поддерживает разбор меток времени с високосными секундами
([8728]) и не учитывает их в вычислениях ([15190]). Если вы сравниваете
два момента времени, разница между ними не будет включать високосные секунды,
которые могли произойти в этом промежутке.

  [8728]: https://github.com/golang/go/issues/8728
  [15190]: https://github.com/golang/go/issues/15190
