# Избегайте «голых» параметров

«Голые» параметры в вызовах функций — значения без пояснений — могут ухудшать
читаемость. Если смысл параметров неочевиден, добавляйте их имена
в комментариях в стиле C (`/* ... */`).

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// func printInfo(name string, isLocal, done bool)

printInfo("foo", true, true)
```

</td><td>

```go
// func printInfo(name string, isLocal, done bool)

printInfo("foo", true /* isLocal */, true /* done */)
```

</td></tr>
</tbody></table>

А ещё лучше — замените «голые» типы `bool` собственными типами: код станет
читабельнее и типобезопаснее. Кроме того, в будущем у такого параметра
может появиться больше двух состояний (true/false).

```go
type Region int

const (
  UnknownRegion Region = iota
  Local
)

type Status int

const (
  StatusReady Status = iota + 1
  StatusDone
  // Возможно, в будущем появится StatusInProgress.
)

func printInfo(name string, region Region, status Status)
```
