# Завершайте программу в main

Программы на Go используют [`os.Exit`] или [`log.Fatal*`] для немедленного
завершения. (Паника — плохой способ завершить программу,
пожалуйста, [не паникуйте](panic.md).)

  [`os.Exit`]: https://pkg.go.dev/os#Exit
  [`log.Fatal*`]: https://pkg.go.dev/log#Fatal

Вызывайте `os.Exit` или `log.Fatal*` **только в `main()`**. Все остальные
функции должны сообщать о сбое, возвращая ошибку.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func main() {
  body := readFile(path)
  fmt.Println(body)
}

func readFile(path string) string {
  f, err := os.Open(path)
  if err != nil {
    log.Fatal(err)
  }

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  return string(b)
}
```

</td><td>

```go
func main() {
  body, err := readFile(path)
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println(body)
}

func readFile(path string) (string, error) {
  f, err := os.Open(path)
  if err != nil {
    return "", err
  }

  b, err := io.ReadAll(f)
  if err != nil {
    return "", err
  }

  return string(b), nil
}
```

</td></tr>
</tbody></table>

Обоснование: у программ, в которых завершать работу могут несколько функций,
есть ряд проблем:

- Неочевидный поток управления: программу может завершить любая функция,
  поэтому рассуждать о потоке управления становится сложно.
- Сложность тестирования: функция, завершающая программу, завершит и тест,
  который её вызывает. Такую функцию трудно тестировать, и появляется риск
  пропустить другие тесты, которые `go test` ещё не успел запустить.
- Пропуск очистки: когда функция завершает программу, вызовы, отложенные
  с помощью `defer`, не выполняются. Появляется риск пропустить важные
  операции по освобождению ресурсов.
