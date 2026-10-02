# Табличные тесты

Табличные тесты с [подтестами] — полезный паттерн, позволяющий избежать
дублирования кода, когда основная логика теста повторяется.

Если тестируемую систему нужно проверить в _нескольких условиях_, когда
меняются определённые части входных и выходных данных, следует использовать
табличный тест, чтобы уменьшить избыточность и улучшить читаемость.

  [подтестами]: https://go.dev/blog/subtests

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestSplitHostPort(t *testing.T)

host, port, err := net.SplitHostPort("192.0.2.0:8000")
require.NoError(t, err)
assert.Equal(t, "192.0.2.0", host)
assert.Equal(t, "8000", port)

host, port, err = net.SplitHostPort("192.0.2.0:http")
require.NoError(t, err)
assert.Equal(t, "192.0.2.0", host)
assert.Equal(t, "http", port)

host, port, err = net.SplitHostPort(":8000")
require.NoError(t, err)
assert.Equal(t, "", host)
assert.Equal(t, "8000", port)

host, port, err = net.SplitHostPort("1:8")
require.NoError(t, err)
assert.Equal(t, "1", host)
assert.Equal(t, "8", port)
```

</td><td>

```go
// func TestSplitHostPort(t *testing.T)

tests := []struct{
  give     string
  wantHost string
  wantPort string
}{
  {
    give:     "192.0.2.0:8000",
    wantHost: "192.0.2.0",
    wantPort: "8000",
  },
  {
    give:     "192.0.2.0:http",
    wantHost: "192.0.2.0",
    wantPort: "http",
  },
  {
    give:     ":8000",
    wantHost: "",
    wantPort: "8000",
  },
  {
    give:     "1:8",
    wantHost: "1",
    wantPort: "8",
  },
}

for _, tt := range tests {
  t.Run(tt.give, func(t *testing.T) {
    host, port, err := net.SplitHostPort(tt.give)
    require.NoError(t, err)
    assert.Equal(t, tt.wantHost, host)
    assert.Equal(t, tt.wantPort, port)
  })
}
```

</td></tr>
</tbody></table>

Тестовые таблицы упрощают добавление контекста в сообщения об ошибках,
сокращают дублирование логики и облегчают добавление новых тестовых случаев.

Мы придерживаемся соглашения, что срез структур называется `tests`,
а каждый тестовый случай — `tt`. Кроме того, мы рекомендуем явно обозначать
входные и выходные значения каждого тестового случая префиксами `give` и `want`.

```go
tests := []struct{
  give     string
  wantHost string
  wantPort string
}{
  // ...
}

for _, tt := range tests {
  // ...
}
```

## Избегайте лишней сложности в табличных тестах

Табличные тесты бывает трудно читать и сопровождать, если подтесты содержат
условные проверки или другую ветвящуюся логику. Табличные тесты **НЕ** следует
использовать, если внутри подтестов нужна сложная или условная логика
(то есть сложная логика внутри цикла `for`).

Большие и сложные табличные тесты ухудшают читаемость и сопровождаемость:
читателю теста может быть трудно разобраться в причинах падения.

Такие табличные тесты следует разбивать либо на несколько тестовых таблиц,
либо на несколько отдельных функций `Test...`.

К чему стоит стремиться:

* Сосредоточиться на минимальной единице поведения.
* Минимизировать «глубину теста» и избегать условных проверок (см. ниже).
* Убедиться, что все поля таблицы используются во всех тестах.
* Убедиться, что вся логика теста выполняется для всех случаев таблицы.

«Глубина теста» здесь означает «количество последовательных проверок внутри
теста, каждая из которых требует, чтобы выполнялись предыдущие» (по аналогии
с цикломатической сложностью). Чем «мельче» тесты, тем меньше связей между
проверками и, что важнее, тем меньше вероятность, что эти проверки окажутся
условными.

Конкретно: табличные тесты становятся запутанными и трудночитаемыми, если
в них несколько ветвей выполнения (например, `shouldError`, `expectCall`
и т. п.), много операторов `if` для отдельных ожиданий моков (например,
`shouldCallFoo`) или функции внутри таблицы (например,
`setupMocks func(*FooMock)`).

Однако если тестируется поведение, которое меняется только в зависимости
от входных данных, может быть предпочтительнее сгруппировать похожие случаи
в одном табличном тесте: так лучше видно, как поведение меняется
на разных входных данных, — вместо того чтобы разносить сопоставимые
случаи по отдельным тестам, где их труднее сравнивать.

Если тело теста короткое и простое, допустимо иметь одну ветку для успешных
и неуспешных случаев с полем таблицы вроде `shouldErr`, задающим ожидание ошибки.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func TestComplicatedTable(t *testing.T) {
  tests := []struct {
    give          string
    want          string
    wantErr       error
    shouldCallX   bool
    shouldCallY   bool
    giveXResponse string
    giveXErr      error
    giveYResponse string
    giveYErr      error
  }{
    // ...
  }

  for _, tt := range tests {
    t.Run(tt.give, func(t *testing.T) {
      // настраиваем моки
      ctrl := gomock.NewController(t)
      xMock := xmock.NewMockX(ctrl)
      if tt.shouldCallX {
        xMock.EXPECT().Call().Return(
          tt.giveXResponse, tt.giveXErr,
        )
      }
      yMock := ymock.NewMockY(ctrl)
      if tt.shouldCallY {
        yMock.EXPECT().Call().Return(
          tt.giveYResponse, tt.giveYErr,
        )
      }

      got, err := DoComplexThing(tt.give, xMock, yMock)

      // проверяем результаты
      if tt.wantErr != nil {
        require.ErrorIs(t, err, tt.wantErr)
        return
      }
      require.NoError(t, err)
      assert.Equal(t, tt.want, got)
    })
  }
}
```

</td><td>

```go
func TestShouldCallX(t *testing.T) {
  // настраиваем моки
  ctrl := gomock.NewController(t)
  xMock := xmock.NewMockX(ctrl)
  xMock.EXPECT().Call().Return("XResponse", nil)

  yMock := ymock.NewMockY(ctrl)

  got, err := DoComplexThing("inputX", xMock, yMock)

  require.NoError(t, err)
  assert.Equal(t, "want", got)
}

func TestShouldCallYAndFail(t *testing.T) {
  // настраиваем моки
  ctrl := gomock.NewController(t)
  xMock := xmock.NewMockX(ctrl)

  yMock := ymock.NewMockY(ctrl)
  yMock.EXPECT().Call().Return("YResponse", nil)

  _, err := DoComplexThing("inputY", xMock, yMock)
  assert.EqualError(t, err, "Y failed")
}
```
</td></tr>
</tbody></table>

Такая сложность затрудняет изменение теста, его понимание и доказательство
его корректности.

Строгих правил здесь нет, но при выборе между табличным тестом и отдельными
тестами для разных входных и выходных данных системы всегда в первую очередь
думайте о читаемости и сопровождаемости.

## Параллельные тесты

Параллельные тесты, как и некоторые особые циклы (например, запускающие
горутины или захватывающие ссылки в теле цикла), должны следить за тем,
чтобы переменные цикла внутри его области видимости содержали ожидаемые
значения.

```go
tests := []struct{
  give string
  // ...
}{
  // ...
}

for _, tt := range tests {
  t.Run(tt.give, func(t *testing.T) {
    t.Parallel()
    // ...
  })
}
```

Начиная с Go 1.22 каждая итерация цикла `for` получает собственную копию
переменной `tt`, поэтому код выше корректен и с `t.Parallel()`.
В более старых версиях Go переменную нужно явно объявить в области видимости
итерации (`tt := tt` в начале тела цикла). Иначе большинство или все тесты
получат неожиданное значение `tt` или значение, которое меняется во время
их выполнения.

<!-- TODO: Explain how to use _test packages. -->
