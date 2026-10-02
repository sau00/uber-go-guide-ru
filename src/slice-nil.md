# nil — валидный срез

`nil` — это валидный срез длины 0. Это означает следующее:

- Не возвращайте явно срез нулевой длины. Вместо этого возвращайте `nil`.

  <table>
  <thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  if x == "" {
    return []int{}
  }
  ```

  </td><td>

  ```go
  if x == "" {
    return nil
  }
  ```

  </td></tr>
  </tbody></table>

- Чтобы проверить, пуст ли срез, всегда используйте `len(s) == 0`.
  Не сравнивайте его с `nil`.

  <table>
  <thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  func isEmpty(s []string) bool {
    return s == nil
  }
  ```

  </td><td>

  ```go
  func isEmpty(s []string) bool {
    return len(s) == 0
  }
  ```

  </td></tr>
  </tbody></table>

- Нулевым значением (срезом, объявленным через `var`) можно пользоваться
  сразу, без `make()`.

  <table>
  <thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  nums := []int{}
  // или nums := make([]int, 0)

  if add1 {
    nums = append(nums, 1)
  }

  if add2 {
    nums = append(nums, 2)
  }
  ```

  </td><td>

  ```go
  var nums []int

  if add1 {
    nums = append(nums, 1)
  }

  if add2 {
    nums = append(nums, 2)
  }
  ```

  </td></tr>
  </tbody></table>

Помните: хотя nil-срез валиден, он не эквивалентен выделенному срезу длины 0 —
один равен nil, а другой нет, — и в разных ситуациях (например,
при сериализации) они могут обрабатываться по-разному.
