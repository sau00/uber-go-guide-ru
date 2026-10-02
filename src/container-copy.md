# Копируйте срезы и мапы на границах

Срезы и мапы содержат указатели на лежащие в их основе данные, поэтому будьте
внимательны в ситуациях, когда их нужно копировать.

## Получение срезов и мап

Помните: если вы сохраните ссылку на мапу или срез, полученные в качестве
аргумента, пользователи смогут их изменить.

<table>
<thead><tr><th>Плохо</th> <th>Хорошо</th></tr></thead>
<tbody>
<tr>
<td>

```go
func (d *Driver) SetTrips(trips []Trip) {
  d.trips = trips
}

trips := ...
d1.SetTrips(trips)

// Вы действительно хотели изменить d1.trips?
trips[0] = ...
```

</td>
<td>

```go
func (d *Driver) SetTrips(trips []Trip) {
  d.trips = make([]Trip, len(trips))
  copy(d.trips, trips)
}

trips := ...
d1.SetTrips(trips)

// Теперь можно менять trips[0], не затрагивая d1.trips.
trips[0] = ...
```

</td>
</tr>

</tbody>
</table>

## Возврат срезов и мап

Аналогично, будьте осторожны: изменяя возвращённые мапы или срезы,
пользователи могут получить доступ к внутреннему состоянию.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

// Snapshot возвращает текущую статистику.
func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  return s.counters
}

// snapshot больше не защищён мьютексом, поэтому любой
// доступ к нему подвержен гонкам данных.
snapshot := stats.Snapshot()
```

</td><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  result := make(map[string]int, len(s.counters))
  for k, v := range s.counters {
    result[k] = v
  }
  return result
}

// Теперь snapshot — это копия.
snapshot := stats.Snapshot()
```

</td></tr>
</tbody></table>


