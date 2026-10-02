<!--
  Этот файл сгенерирован stitchmd. НЕ РЕДАКТИРУЙТЕ ЕГО ВРУЧНУЮ.
  Чтобы внести изменения, правьте файлы в каталоге "src".
-->

<!-- markdownlint-disable MD033 -->

> Это русский перевод [Uber Go Style Guide](https://github.com/uber-go/guide/blob/master/style.md).
> Код в примерах соответствует оригиналу: переведены комментарии и исправлено
> несколько опечаток. Примечания переводчика выделены отдельно.
> Если какая-то формулировка кажется неоднозначной, сверяйтесь с оригиналом.

# Руководство по стилю Go от Uber

- [Введение](#%D0%B2%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5)
- [Рекомендации](#рекомендации)
  - [Указатели на интерфейсы](#%D1%83%D0%BA%D0%B0%D0%B7%D0%B0%D1%82%D0%B5%D0%BB%D0%B8-%D0%BD%D0%B0-%D0%B8%D0%BD%D1%82%D0%B5%D1%80%D1%84%D0%B5%D0%B9%D1%81%D1%8B)
  - [Проверяйте соответствие интерфейсу](#%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D1%8F%D0%B9%D1%82%D0%B5-%D1%81%D0%BE%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D1%81%D1%82%D0%B2%D0%B8%D0%B5-%D0%B8%D0%BD%D1%82%D0%B5%D1%80%D1%84%D0%B5%D0%B9%D1%81%D1%83)
  - [Получатели и интерфейсы](#%D0%BF%D0%BE%D0%BB%D1%83%D1%87%D0%B0%D1%82%D0%B5%D0%BB%D0%B8-%D0%B8-%D0%B8%D0%BD%D1%82%D0%B5%D1%80%D1%84%D0%B5%D0%B9%D1%81%D1%8B)
  - [Нулевое значение мьютекса валидно](#%D0%BD%D1%83%D0%BB%D0%B5%D0%B2%D0%BE%D0%B5-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BC%D1%8C%D1%8E%D1%82%D0%B5%D0%BA%D1%81%D0%B0-%D0%B2%D0%B0%D0%BB%D0%B8%D0%B4%D0%BD%D0%BE)
  - [Копируйте срезы и мапы на границах](#%D0%BA%D0%BE%D0%BF%D0%B8%D1%80%D1%83%D0%B9%D1%82%D0%B5-%D1%81%D1%80%D0%B5%D0%B7%D1%8B-%D0%B8-%D0%BC%D0%B0%D0%BF%D1%8B-%D0%BD%D0%B0-%D0%B3%D1%80%D0%B0%D0%BD%D0%B8%D1%86%D0%B0%D1%85)
  - [Используйте defer для освобождения ресурсов](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-defer-%D0%B4%D0%BB%D1%8F-%D0%BE%D1%81%D0%B2%D0%BE%D0%B1%D0%BE%D0%B6%D0%B4%D0%B5%D0%BD%D0%B8%D1%8F-%D1%80%D0%B5%D1%81%D1%83%D1%80%D1%81%D0%BE%D0%B2)
  - [Размер канала — один или ноль](#%D1%80%D0%B0%D0%B7%D0%BC%D0%B5%D1%80-%D0%BA%D0%B0%D0%BD%D0%B0%D0%BB%D0%B0--%D0%BE%D0%B4%D0%B8%D0%BD-%D0%B8%D0%BB%D0%B8-%D0%BD%D0%BE%D0%BB%D1%8C)
  - [Начинайте перечисления с единицы](#%D0%BD%D0%B0%D1%87%D0%B8%D0%BD%D0%B0%D0%B9%D1%82%D0%B5-%D0%BF%D0%B5%D1%80%D0%B5%D1%87%D0%B8%D1%81%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F-%D1%81-%D0%B5%D0%B4%D0%B8%D0%BD%D0%B8%D1%86%D1%8B)
  - [Используйте пакет `"time"` для работы со временем](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-%D0%BF%D0%B0%D0%BA%D0%B5%D1%82-time-%D0%B4%D0%BB%D1%8F-%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%8B-%D1%81%D0%BE-%D0%B2%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%B5%D0%BC)
  - [Ошибки](#ошибки)
    - [Типы ошибок](#%D1%82%D0%B8%D0%BF%D1%8B-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA)
    - [Оборачивание ошибок](#%D0%BE%D0%B1%D0%BE%D1%80%D0%B0%D1%87%D0%B8%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA)
    - [Именование ошибок](#%D0%B8%D0%BC%D0%B5%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA)
    - [Обрабатывайте ошибку один раз](#%D0%BE%D0%B1%D1%80%D0%B0%D0%B1%D0%B0%D1%82%D1%8B%D0%B2%D0%B0%D0%B9%D1%82%D0%B5-%D0%BE%D1%88%D0%B8%D0%B1%D0%BA%D1%83-%D0%BE%D0%B4%D0%B8%D0%BD-%D1%80%D0%B0%D0%B7)
  - [Обрабатывайте неудачные приведения типов](#%D0%BE%D0%B1%D1%80%D0%B0%D0%B1%D0%B0%D1%82%D1%8B%D0%B2%D0%B0%D0%B9%D1%82%D0%B5-%D0%BD%D0%B5%D1%83%D0%B4%D0%B0%D1%87%D0%BD%D1%8B%D0%B5-%D0%BF%D1%80%D0%B8%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D1%8F-%D1%82%D0%B8%D0%BF%D0%BE%D0%B2)
  - [Не паникуйте](#%D0%BD%D0%B5-%D0%BF%D0%B0%D0%BD%D0%B8%D0%BA%D1%83%D0%B9%D1%82%D0%B5)
  - [Используйте go.uber.org/atomic](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-gouberorgatomic)
  - [Избегайте изменяемых глобальных переменных](#%D0%B8%D0%B7%D0%B1%D0%B5%D0%B3%D0%B0%D0%B9%D1%82%D0%B5-%D0%B8%D0%B7%D0%BC%D0%B5%D0%BD%D1%8F%D0%B5%D0%BC%D1%8B%D1%85-%D0%B3%D0%BB%D0%BE%D0%B1%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85)
  - [Не встраивайте типы в публичные структуры](#%D0%BD%D0%B5-%D0%B2%D1%81%D1%82%D1%80%D0%B0%D0%B8%D0%B2%D0%B0%D0%B9%D1%82%D0%B5-%D1%82%D0%B8%D0%BF%D1%8B-%D0%B2-%D0%BF%D1%83%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D1%8B)
  - [Не используйте имена встроенных идентификаторов](#%D0%BD%D0%B5-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-%D0%B8%D0%BC%D0%B5%D0%BD%D0%B0-%D0%B2%D1%81%D1%82%D1%80%D0%BE%D0%B5%D0%BD%D0%BD%D1%8B%D1%85-%D0%B8%D0%B4%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%82%D0%BE%D1%80%D0%BE%D0%B2)
  - [Избегайте `init()`](#%D0%B8%D0%B7%D0%B1%D0%B5%D0%B3%D0%B0%D0%B9%D1%82%D0%B5-init)
  - [Завершайте программу в main](#%D0%B7%D0%B0%D0%B2%D0%B5%D1%80%D1%88%D0%B0%D0%B9%D1%82%D0%B5-%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D1%83-%D0%B2-main)
    - [Завершайте программу один раз](#%D0%B7%D0%B0%D0%B2%D0%B5%D1%80%D1%88%D0%B0%D0%B9%D1%82%D0%B5-%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D1%83-%D0%BE%D0%B4%D0%B8%D0%BD-%D1%80%D0%B0%D0%B7)
  - [Используйте теги полей в сериализуемых структурах](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-%D1%82%D0%B5%D0%B3%D0%B8-%D0%BF%D0%BE%D0%BB%D0%B5%D0%B9-%D0%B2-%D1%81%D0%B5%D1%80%D0%B8%D0%B0%D0%BB%D0%B8%D0%B7%D1%83%D0%B5%D0%BC%D1%8B%D1%85-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D0%B0%D1%85)
  - [Не запускайте горутины по принципу «запустил и забыл»](#%D0%BD%D0%B5-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%D0%B0%D0%B9%D1%82%D0%B5-%D0%B3%D0%BE%D1%80%D1%83%D1%82%D0%B8%D0%BD%D1%8B-%D0%BF%D0%BE-%D0%BF%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF%D1%83-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D1%82%D0%B8%D0%BB-%D0%B8-%D0%B7%D0%B0%D0%B1%D1%8B%D0%BB)
    - [Дожидайтесь завершения горутин](#%D0%B4%D0%BE%D0%B6%D0%B8%D0%B4%D0%B0%D0%B9%D1%82%D0%B5%D1%81%D1%8C-%D0%B7%D0%B0%D0%B2%D0%B5%D1%80%D1%88%D0%B5%D0%BD%D0%B8%D1%8F-%D0%B3%D0%BE%D1%80%D1%83%D1%82%D0%B8%D0%BD)
    - [Не запускайте горутины в `init()`](#%D0%BD%D0%B5-%D0%B7%D0%B0%D0%BF%D1%83%D1%81%D0%BA%D0%B0%D0%B9%D1%82%D0%B5-%D0%B3%D0%BE%D1%80%D1%83%D1%82%D0%B8%D0%BD%D1%8B-%D0%B2-init)
- [Производительность](#%D0%BF%D1%80%D0%BE%D0%B8%D0%B7%D0%B2%D0%BE%D0%B4%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE%D1%81%D1%82%D1%8C)
  - [Предпочитайте strconv вместо fmt](#%D0%BF%D1%80%D0%B5%D0%B4%D0%BF%D0%BE%D1%87%D0%B8%D1%82%D0%B0%D0%B9%D1%82%D0%B5-strconv-%D0%B2%D0%BC%D0%B5%D1%81%D1%82%D0%BE-fmt)
  - [Избегайте повторных преобразований строки в срез байтов](#%D0%B8%D0%B7%D0%B1%D0%B5%D0%B3%D0%B0%D0%B9%D1%82%D0%B5-%D0%BF%D0%BE%D0%B2%D1%82%D0%BE%D1%80%D0%BD%D1%8B%D1%85-%D0%BF%D1%80%D0%B5%D0%BE%D0%B1%D1%80%D0%B0%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B9-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B8-%D0%B2-%D1%81%D1%80%D0%B5%D0%B7-%D0%B1%D0%B0%D0%B9%D1%82%D0%BE%D0%B2)
  - [Указывайте ёмкость контейнеров](#%D1%83%D0%BA%D0%B0%D0%B7%D1%8B%D0%B2%D0%B0%D0%B9%D1%82%D0%B5-%D1%91%D0%BC%D0%BA%D0%BE%D1%81%D1%82%D1%8C-%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D0%BE%D0%B2)
- [Стиль](#стиль)
  - [Избегайте слишком длинных строк](#%D0%B8%D0%B7%D0%B1%D0%B5%D0%B3%D0%B0%D0%B9%D1%82%D0%B5-%D1%81%D0%BB%D0%B8%D1%88%D0%BA%D0%BE%D0%BC-%D0%B4%D0%BB%D0%B8%D0%BD%D0%BD%D1%8B%D1%85-%D1%81%D1%82%D1%80%D0%BE%D0%BA)
  - [Будьте последовательны](#%D0%B1%D1%83%D0%B4%D1%8C%D1%82%D0%B5-%D0%BF%D0%BE%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B)
  - [Группируйте похожие объявления](#%D0%B3%D1%80%D1%83%D0%BF%D0%BF%D0%B8%D1%80%D1%83%D0%B9%D1%82%D0%B5-%D0%BF%D0%BE%D1%85%D0%BE%D0%B6%D0%B8%D0%B5-%D0%BE%D0%B1%D1%8A%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F)
  - [Порядок групп импорта](#%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BE%D0%BA-%D0%B3%D1%80%D1%83%D0%BF%D0%BF-%D0%B8%D0%BC%D0%BF%D0%BE%D1%80%D1%82%D0%B0)
  - [Имена пакетов](#%D0%B8%D0%BC%D0%B5%D0%BD%D0%B0-%D0%BF%D0%B0%D0%BA%D0%B5%D1%82%D0%BE%D0%B2)
  - [Имена функций](#%D0%B8%D0%BC%D0%B5%D0%BD%D0%B0-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B9)
  - [Псевдонимы импортов](#%D0%BF%D1%81%D0%B5%D0%B2%D0%B4%D0%BE%D0%BD%D0%B8%D0%BC%D1%8B-%D0%B8%D0%BC%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2)
  - [Группировка и порядок функций](#%D0%B3%D1%80%D1%83%D0%BF%D0%BF%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0-%D0%B8-%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BE%D0%BA-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B9)
  - [Уменьшайте вложенность](#%D1%83%D0%BC%D0%B5%D0%BD%D1%8C%D1%88%D0%B0%D0%B9%D1%82%D0%B5-%D0%B2%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D1%8C)
  - [Лишний else](#%D0%BB%D0%B8%D1%88%D0%BD%D0%B8%D0%B9-else)
  - [Объявление переменных верхнего уровня](#%D0%BE%D0%B1%D1%8A%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85-%D0%B2%D0%B5%D1%80%D1%85%D0%BD%D0%B5%D0%B3%D0%BE-%D1%83%D1%80%D0%BE%D0%B2%D0%BD%D1%8F)
  - [Используйте префикс _ для неэкспортируемых глобальных переменных](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-%D0%BF%D1%80%D0%B5%D1%84%D0%B8%D0%BA%D1%81-_-%D0%B4%D0%BB%D1%8F-%D0%BD%D0%B5%D1%8D%D0%BA%D1%81%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D1%83%D0%B5%D0%BC%D1%8B%D1%85-%D0%B3%D0%BB%D0%BE%D0%B1%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85)
  - [Встраивание в структуры](#%D0%B2%D1%81%D1%82%D1%80%D0%B0%D0%B8%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%B2-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D1%8B)
  - [Объявление локальных переменных](#%D0%BE%D0%B1%D1%8A%D1%8F%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BB%D0%BE%D0%BA%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85)
  - [nil — валидный срез](#nil--%D0%B2%D0%B0%D0%BB%D0%B8%D0%B4%D0%BD%D1%8B%D0%B9-%D1%81%D1%80%D0%B5%D0%B7)
  - [Сужайте область видимости переменных](#%D1%81%D1%83%D0%B6%D0%B0%D0%B9%D1%82%D0%B5-%D0%BE%D0%B1%D0%BB%D0%B0%D1%81%D1%82%D1%8C-%D0%B2%D0%B8%D0%B4%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D0%B8-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85)
  - [Избегайте «голых» параметров](#%D0%B8%D0%B7%D0%B1%D0%B5%D0%B3%D0%B0%D0%B9%D1%82%D0%B5-%D0%B3%D0%BE%D0%BB%D1%8B%D1%85-%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%BE%D0%B2)
  - [Используйте сырые строковые литералы, чтобы избежать экранирования](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-%D1%81%D1%8B%D1%80%D1%8B%D0%B5-%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%BE%D0%B2%D1%8B%D0%B5-%D0%BB%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D0%BB%D1%8B-%D1%87%D1%82%D0%BE%D0%B1%D1%8B-%D0%B8%D0%B7%D0%B1%D0%B5%D0%B6%D0%B0%D1%82%D1%8C-%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F)
  - [Инициализация структур](#инициализация-структур)
    - [Используйте имена полей при инициализации структур](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-%D0%B8%D0%BC%D0%B5%D0%BD%D0%B0-%D0%BF%D0%BE%D0%BB%D0%B5%D0%B9-%D0%BF%D1%80%D0%B8-%D0%B8%D0%BD%D0%B8%D1%86%D0%B8%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D0%B8-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80)
    - [Опускайте поля с нулевыми значениями](#%D0%BE%D0%BF%D1%83%D1%81%D0%BA%D0%B0%D0%B9%D1%82%D0%B5-%D0%BF%D0%BE%D0%BB%D1%8F-%D1%81-%D0%BD%D1%83%D0%BB%D0%B5%D0%B2%D1%8B%D0%BC%D0%B8-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D1%8F%D0%BC%D0%B8)
    - [Используйте `var` для структур с нулевым значением](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-var-%D0%B4%D0%BB%D1%8F-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80-%D1%81-%D0%BD%D1%83%D0%BB%D0%B5%D0%B2%D1%8B%D0%BC-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5%D0%BC)
    - [Инициализация ссылок на структуры](#%D0%B8%D0%BD%D0%B8%D1%86%D0%B8%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F-%D1%81%D1%81%D1%8B%D0%BB%D0%BE%D0%BA-%D0%BD%D0%B0-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D1%8B)
  - [Инициализация мап](#%D0%B8%D0%BD%D0%B8%D1%86%D0%B8%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F-%D0%BC%D0%B0%D0%BF)
  - [Строки формата вне Printf](#%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B8-%D1%84%D0%BE%D1%80%D0%BC%D0%B0%D1%82%D0%B0-%D0%B2%D0%BD%D0%B5-printf)
  - [Именование функций в стиле Printf](#%D0%B8%D0%BC%D0%B5%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B9-%D0%B2-%D1%81%D1%82%D0%B8%D0%BB%D0%B5-printf)
- [Паттерны](#паттерны)
  - [Табличные тесты](#%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5-%D1%82%D0%B5%D1%81%D1%82%D1%8B)
  - [Функциональные опции](#%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%BE%D0%BD%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5-%D0%BE%D0%BF%D1%86%D0%B8%D0%B8)
- [Линтинг](#%D0%BB%D0%B8%D0%BD%D1%82%D0%B8%D0%BD%D0%B3)

## Введение

Стиль — это соглашения, которым подчиняется наш код. Термин «стиль» здесь
не вполне точен: эти соглашения охватывают гораздо больше, чем форматирование
исходных файлов, — с форматированием за нас прекрасно справляется gofmt.

Цель этого руководства — справиться с этой сложностью, подробно описав,
что стоит и чего не стоит делать при написании кода на Go в Uber. Эти правила
существуют, чтобы кодовая база оставалась управляемой и при этом инженеры
могли продуктивно пользоваться возможностями языка Go.

Изначально руководство было создано [Прашантом Варанаси](https://github.com/prashantv) и [Саймоном Ньютоном](https://github.com/nomis52),
чтобы помочь коллегам быстрее освоиться с Go. С годами оно дополнялось
на основе отзывов других людей.

Здесь описаны идиоматичные соглашения для кода на Go, которым мы следуем в Uber.
Многие из них — общие рекомендации для Go, другие же дополняют внешние источники:

1. [Effective Go](https://go.dev/doc/effective_go)
2. [Go Common Mistakes](https://go.dev/wiki/CommonMistakes)
3. [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments)

Мы стремимся к тому, чтобы примеры кода были корректны для двух последних
минорных [релизов](https://go.dev/doc/devel/release) Go.

Весь код должен проходить проверки `golint` и `go vet` без ошибок.
Рекомендуем настроить редактор так, чтобы он:

- запускал `goimports` при сохранении;
- запускал `golint` и `go vet` для поиска ошибок.

> **Примечание переводчика.** `golint` объявлен устаревшим; его современная
> замена — [revive](https://github.com/mgechev/revive). Подробнее
> см. раздел [Линтинг](#%D0%BB%D0%B8%D0%BD%D1%82%D0%B8%D0%BD%D0%B3).

Информацию о поддержке инструментов Go в редакторах можно найти здесь:
https://go.dev/wiki/IDEsAndTextEditorPlugins

## Рекомендации

### Указатели на интерфейсы

Указатель на интерфейс вам почти никогда не понадобится. Интерфейсы следует
передавать по значению — при этом лежащие в их основе данные всё равно могут
быть указателем.

Интерфейс состоит из двух полей:

1. Указатель на информацию о конкретном типе. Можно считать, что это
   «тип».
2. Указатель на данные. Если хранимые данные — указатель, он хранится
   напрямую. Если хранимые данные — значение, хранится указатель на это значение.

Если вы хотите, чтобы методы интерфейса изменяли лежащие в его основе данные,
необходимо использовать указатель.

### Проверяйте соответствие интерфейсу

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

### Получатели и интерфейсы

Методы с получателем по значению можно вызывать как у значений,
так и у указателей. Методы с получателем по указателю можно вызывать только
у указателей или [адресуемых значений](https://go.dev/ref/spec#Method_values).

Например:

```go
type S struct {
  data string
}

func (s S) Read() string {
  return s.data
}

func (s *S) Write(str string) {
  s.data = str
}

// Мы не можем получить указатели на значения, хранящиеся в мапе,
// потому что они не являются адресуемыми.
sVals := map[int]S{1: {"A"}}

// Read можно вызвать у значений из мапы, потому что у Read
// получатель по значению, а он не требует адресуемости.
sVals[1].Read()

// Write нельзя вызвать у значений из мапы, потому что у Write
// получатель по указателю, а получить указатель на значение
// из мапы невозможно.
//
//  sVals[1].Write("test")

sPtrs := map[int]*S{1: {"A"}}

// Если мапа хранит указатели, можно вызывать и Read, и Write,
// потому что указатели адресуемы по своей природе.
sPtrs[1].Read()
sPtrs[1].Write("test")
```

Аналогично, интерфейс может быть реализован указателем, даже если у метода
получатель по значению.

```go
type F interface {
  f()
}

type S1 struct{}

func (s S1) f() {}

type S2 struct{}

func (s *S2) f() {}

s1Val := S1{}
s1Ptr := &S1{}
s2Val := S2{}
s2Ptr := &S2{}

var i F
i = s1Val
i = s1Ptr
i = s2Ptr

// Следующая строка не скомпилируется: s2Val — значение, а у f нет получателя по значению.
//   i = s2Val
```

В Effective Go есть хорошая статья на эту тему: [Pointers vs. Values](https://go.dev/doc/effective_go#pointers_vs_values).

### Нулевое значение мьютекса валидно

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

### Копируйте срезы и мапы на границах

Срезы и мапы содержат указатели на лежащие в их основе данные, поэтому будьте
внимательны в ситуациях, когда их нужно копировать.

#### Получение срезов и мап

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

#### Возврат срезов и мап

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

### Используйте defer для освобождения ресурсов

Используйте `defer` для освобождения ресурсов, таких как файлы и блокировки.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
p.Lock()
if p.count < 10 {
  p.Unlock()
  return p.count
}

p.count++
newCount := p.count
p.Unlock()

return newCount

// из-за нескольких return легко
// пропустить разблокировку
```

</td><td>

```go
p.Lock()
defer p.Unlock()

if p.count < 10 {
  return p.count
}

p.count++
return p.count

// читается легче
```

</td></tr>
</tbody></table>

Накладные расходы `defer` крайне малы, и избегать его стоит только в том случае,
если вы можете доказать, что время выполнения вашей функции измеряется
наносекундами. Выигрыш в читаемости от использования `defer` стоит этой
ничтожной цены. Особенно это касается крупных методов, которые делают больше,
чем простые обращения к памяти: там остальные вычисления обходятся гораздо
дороже, чем `defer`.

### Размер канала — один или ноль

Как правило, каналы должны иметь размер 1 или быть небуферизованными.
По умолчанию каналы небуферизованные и имеют размер 0. Любой другой размер
требует самого пристального внимания. Подумайте, как определяется размер,
что помешает каналу заполниться под нагрузкой и заблокировать пишущих в него,
и что произойдёт, если это всё-таки случится.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// Этого хватит кому угодно!
c := make(chan int, 64)
```

</td><td>

```go
// Размер 1
c := make(chan int, 1) // или
// Небуферизованный канал, размер 0
c := make(chan int)
```

</td></tr>
</tbody></table>

### Начинайте перечисления с единицы

Стандартный способ объявить перечисление в Go — объявить собственный тип
и группу `const` с `iota`. Поскольку значение переменных по умолчанию — 0,
обычно перечисления стоит начинать с ненулевого значения.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type Operation int

const (
  Add Operation = iota
  Subtract
  Multiply
)

// Add=0, Subtract=1, Multiply=2
```

</td><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)

// Add=1, Subtract=2, Multiply=3
```

</td></tr>
</tbody></table>

Бывают случаи, когда использование нулевого значения оправдано, например,
когда вариант с нулевым значением — желаемое поведение по умолчанию.

```go
type LogOutput int

const (
  LogToStdout LogOutput = iota
  LogToFile
  LogToRemote
)

// LogToStdout=0, LogToFile=1, LogToRemote=2
```

<!-- TODO: section on String methods for enums -->

### Используйте пакет `"time"` для работы со временем

Время — сложная штука. Вот некоторые неверные предположения о времени,
которые часто делают:

1. В сутках 24 часа
2. В часе 60 минут
3. В неделе 7 дней
4. В году 365 дней
5. [И многое другое](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)

Например, из пункта *1* следует, что прибавление 24 часов к моменту времени
не всегда даёт новый календарный день.

Поэтому для работы со временем всегда используйте пакет [`"time"`](https://pkg.go.dev/time): он помогает
справляться с этими неверными предположениями безопаснее и точнее.

#### Используйте `time.Time` для моментов времени

Используйте [`time.Time`](https://pkg.go.dev/time#Time) для работы с моментами времени, а для сравнения,
сложения и вычитания времени — методы `time.Time`.

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

#### Используйте `time.Duration` для промежутков времени

Используйте [`time.Duration`](https://pkg.go.dev/time#Duration) для работы с промежутками времени.

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
но на следующий календарный день, следует использовать [`Time.AddDate`](https://pkg.go.dev/time#Time.AddDate).
Если же нужен момент времени, гарантированно отстоящий от предыдущего
ровно на 24 часа, следует использовать [`Time.Add`](https://pkg.go.dev/time#Time.Add).

```go
newDay := t.AddDate(0 /* годы */, 0 /* месяцы */, 1 /* дни */)
maybeNewDay := t.Add(24 * time.Hour)
```

#### Используйте `time.Time` и `time.Duration` при взаимодействии с внешними системами

По возможности используйте `time.Duration` и `time.Time` при взаимодействии
с внешними системами. Например:

- Флаги командной строки: [`flag`](https://pkg.go.dev/flag) поддерживает `time.Duration` с помощью
  [`time.ParseDuration`](https://pkg.go.dev/time#ParseDuration)
- JSON: [`encoding/json`](https://pkg.go.dev/encoding/json) поддерживает кодирование `time.Time` в строку
  формата [RFC 3339](https://tools.ietf.org/html/rfc3339) благодаря [методу `UnmarshalJSON`](https://pkg.go.dev/time#Time.UnmarshalJSON)
- SQL: [`database/sql`](https://pkg.go.dev/database/sql) поддерживает преобразование столбцов `DATETIME`
  и `TIMESTAMP` в `time.Time` и обратно, если это поддерживает драйвер
- YAML: [`gopkg.in/yaml.v2`](https://pkg.go.dev/gopkg.in/yaml.v2) поддерживает `time.Time` в виде строки формата
  [RFC 3339](https://tools.ietf.org/html/rfc3339) и `time.Duration` с помощью [`time.ParseDuration`](https://pkg.go.dev/time#ParseDuration).

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
согласно [RFC 3339](https://tools.ietf.org/html/rfc3339). Этот формат по умолчанию используется в
[`Time.UnmarshalText`](https://pkg.go.dev/time#Time.UnmarshalText) и доступен для `Time.Format` и `time.Parse`
через константу [`time.RFC3339`](https://pkg.go.dev/time#RFC3339).

Хотя на практике это обычно не вызывает проблем, имейте в виду, что пакет
`"time"` не поддерживает разбор меток времени с високосными секундами
([8728](https://github.com/golang/go/issues/8728)) и не учитывает их в вычислениях ([15190](https://github.com/golang/go/issues/15190)). Если вы сравниваете
два момента времени, разница между ними не будет включать високосные секунды,
которые могли произойти в этом промежутке.

### Ошибки

#### Типы ошибок

Есть несколько способов объявить ошибку.
Прежде чем выбрать наиболее подходящий для вашего случая, ответьте на вопросы:

- Нужно ли вызывающему коду распознавать ошибку, чтобы обработать её?
  Если да, нужно поддержать функции [`errors.Is`](https://pkg.go.dev/errors#Is) или [`errors.As`](https://pkg.go.dev/errors#As),
  объявив переменную ошибки верхнего уровня или собственный тип.
- Сообщение об ошибке — статическая строка
  или динамическая, требующая контекстной информации?
  В первом случае можно использовать [`errors.New`](https://pkg.go.dev/errors#New), во втором —
  [`fmt.Errorf`](https://pkg.go.dev/fmt#Errorf) или собственный тип ошибки.
- Передаём ли мы дальше новую ошибку, полученную от нижележащей функции?
  Если да, смотрите [раздел об оборачивании ошибок](#%D0%BE%D0%B1%D0%BE%D1%80%D0%B0%D1%87%D0%B8%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA).

| Распознавание ошибки? | Сообщение    | Рекомендация                                                          |
|-----------------------|--------------|-----------------------------------------------------------------------|
| Нет                   | статическое  | [`errors.New`](https://pkg.go.dev/errors#New)                         |
| Нет                   | динамическое | [`fmt.Errorf`](https://pkg.go.dev/fmt#Errorf)                         |
| Да                    | статическое  | `var` верхнего уровня с [`errors.New`](https://pkg.go.dev/errors#New) |
| Да                    | динамическое | собственный тип `error`                                               |

Например, для ошибки со статической строкой используйте [`errors.New`](https://pkg.go.dev/errors#New).
Если вызывающему коду нужно распознавать и обрабатывать эту ошибку,
экспортируйте её как переменную, чтобы её можно было проверить с помощью `errors.Is`.

<table>
<thead><tr><th>Без распознавания ошибки</th><th>С распознаванием ошибки</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open() error {
  return errors.New("could not open")
}

// package bar

if err := foo.Open(); err != nil {
  // Обработать ошибку невозможно.
  panic("unknown error")
}
```

</td><td>

```go
// package foo

var ErrCouldNotOpen = errors.New("could not open")

func Open() error {
  return ErrCouldNotOpen
}

// package bar

if err := foo.Open(); err != nil {
  if errors.Is(err, foo.ErrCouldNotOpen) {
    // обрабатываем ошибку
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Для ошибки с динамической строкой используйте [`fmt.Errorf`](https://pkg.go.dev/fmt#Errorf), если вызывающему
коду не нужно её распознавать, и собственный тип `error`, если нужно.

<table>
<thead><tr><th>Без распознавания ошибки</th><th>С распознаванием ошибки</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open(file string) error {
  return fmt.Errorf("file %q not found", file)
}

// package bar

if err := foo.Open("testfile.txt"); err != nil {
  // Обработать ошибку невозможно.
  panic("unknown error")
}
```

</td><td>

```go
// package foo

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

func Open(file string) error {
  return &NotFoundError{File: file}
}


// package bar

if err := foo.Open("testfile.txt"); err != nil {
  var notFound *NotFoundError
  if errors.As(err, &notFound) {
    // обрабатываем ошибку
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

Учтите, что экспортируемые из пакета переменные и типы ошибок
становятся частью его публичного API.

#### Оборачивание ошибок

Если вызов завершился ошибкой, есть три основных способа передать её дальше:

- вернуть исходную ошибку как есть;
- добавить контекст с помощью `fmt.Errorf` и глагола `%w`;
- добавить контекст с помощью `fmt.Errorf` и глагола `%v`.

Возвращайте исходную ошибку как есть, если добавить к ней нечего.
Так сохраняются исходные тип и сообщение ошибки.
Это хорошо подходит для случаев, когда сообщения исходной ошибки
достаточно, чтобы понять, откуда она взялась.

В остальных случаях по возможности добавляйте к сообщению контекст,
чтобы вместо расплывчатой ошибки вроде «connection refused»
получать более полезные, например «call service foo: connection refused».

Добавляйте контекст к ошибкам с помощью `fmt.Errorf`, выбирая между глаголами
`%w` и `%v` в зависимости от того, должен ли вызывающий код иметь возможность
распознать и извлечь исходную причину.

- Используйте `%w`, если вызывающий код должен иметь доступ к исходной ошибке.
  Это хороший вариант по умолчанию для большинства обёрнутых ошибок,
  но учтите, что вызывающий код может начать полагаться на это поведение.
  Поэтому если обёрнутая ошибка — известная переменная (`var`) или тип,
  документируйте и тестируйте это как часть контракта функции.
- Используйте `%v`, чтобы скрыть исходную ошибку.
  Вызывающий код не сможет её распознать,
  но при необходимости в будущем можно перейти на `%w`.

Добавляя контекст к возвращаемым ошибкам, делайте его лаконичным и избегайте
фраз вроде «failed to»: они сообщают очевидное и накапливаются по мере того,
как ошибка поднимается по стеку:

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "failed to create new store: %w", err)
}
```

</td><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "new store: %w", err)
}
```

</td></tr><tr><td>

```plain
failed to x: failed to y: failed to create new store: the error
```

</td><td>

```plain
x: y: new store: the error
```

</td></tr>
</tbody></table>

Однако когда ошибка передаётся в другую систему, должно быть понятно,
что это именно ошибка (например, по тегу `err` или префиксу «Failed» в логах).

См. также [Don't just check errors, handle them gracefully](https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully).

#### Именование ошибок

Для значений ошибок, хранящихся в глобальных переменных, используйте префикс
`Err` или `err` в зависимости от того, экспортируются ли они.
Эта рекомендация имеет приоритет над правилом
[Используйте префикс _ для неэкспортируемых глобальных переменных](#%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D1%83%D0%B9%D1%82%D0%B5-%D0%BF%D1%80%D0%B5%D1%84%D0%B8%D0%BA%D1%81-_-%D0%B4%D0%BB%D1%8F-%D0%BD%D0%B5%D1%8D%D0%BA%D1%81%D0%BF%D0%BE%D1%80%D1%82%D0%B8%D1%80%D1%83%D0%B5%D0%BC%D1%8B%D1%85-%D0%B3%D0%BB%D0%BE%D0%B1%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85-%D0%BF%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D1%8B%D1%85).

```go
var (
  // Следующие две ошибки экспортируются,
  // чтобы пользователи пакета могли распознавать их
  // с помощью errors.Is.

  ErrBrokenLink = errors.New("link is broken")
  ErrCouldNotOpen = errors.New("could not open")

  // Эта ошибка не экспортируется, потому что
  // мы не хотим делать её частью публичного API.
  // Внутри пакета её всё равно можно использовать
  // с errors.Is.

  errNotFound = errors.New("not found")
)
```

Для собственных типов ошибок используйте суффикс `Error`.

```go
// Эта ошибка тоже экспортируется,
// чтобы пользователи пакета могли распознавать её
// с помощью errors.As.

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

// А эта ошибка не экспортируется, потому что
// мы не хотим делать её частью публичного API.
// Внутри пакета её всё равно можно использовать
// с errors.As.

type resolveError struct {
  Path string
}

func (e *resolveError) Error() string {
  return fmt.Sprintf("resolve %q", e.Path)
}
```

#### Обрабатывайте ошибку один раз

Когда вызывающий код получает ошибку от вызываемого,
он может обработать её по-разному — в зависимости от того,
что ему об этой ошибке известно.

В том числе (но не только):

- если контракт вызываемой функции определяет конкретные ошибки —
  распознать ошибку с помощью `errors.Is` или `errors.As`
  и обработать разные варианты по-разному;
- если после ошибки можно восстановиться —
  записать её в лог и мягко деградировать;
- если ошибка означает сбой, специфичный для предметной области, —
  вернуть чётко определённую ошибку;
- вернуть ошибку — [обёрнутой](#%D0%BE%D0%B1%D0%BE%D1%80%D0%B0%D1%87%D0%B8%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA) или как есть.

Как бы вызывающий код ни обрабатывал ошибку,
обычно каждую ошибку следует обрабатывать только один раз.
Например, не стоит записывать ошибку в лог и затем возвращать её:
*его собственный* вызывающий код тоже может её обработать.

Рассмотрим следующие случаи:

<table>
<thead><tr><th>Описание</th><th>Код</th></tr></thead>
<tbody>
<tr><td>

**Плохо**: записать ошибку в лог и вернуть её

Код выше по стеку, скорее всего, поступит с ошибкой так же.
Это создаёт много шума в логах приложения при небольшой пользе.

</td><td>

```go
u, err := getUser(id)
if err != nil {
  // ПЛОХО: см. описание
  log.Printf("Could not get user %q: %v", id, err)
  return err
}
```

</td></tr>
<tr><td>

**Хорошо**: обернуть ошибку и вернуть её

Код выше по стеку обработает ошибку.
Использование `%w` позволяет ему при необходимости распознать ошибку
с помощью `errors.Is` или `errors.As`.

</td><td>

```go
u, err := getUser(id)
if err != nil {
  return fmt.Errorf("get user %q: %w", id, err)
}
```

</td></tr>
<tr><td>

**Хорошо**: записать ошибку в лог и мягко деградировать

Если операция не строго обязательна,
можно восстановиться после ошибки и продолжить работу
в урезанном, но не сломанном режиме.

</td><td>

```go
if err := emitMetrics(); err != nil {
  // Ошибка записи метрик не должна
  // ломать приложение.
  log.Printf("Could not emit metrics: %v", err)
}

```

</td></tr>
<tr><td>

**Хорошо**: распознать ошибку и мягко деградировать

Если вызываемая функция определяет в своём контракте конкретную ошибку
и после сбоя можно восстановиться,
обработайте этот случай и мягко деградируйте.
Во всех остальных случаях оберните ошибку и верните её.

Остальные ошибки обработает код выше по стеку.

</td><td>

```go
tz, err := getUserTimeZone(id)
if err != nil {
  if errors.Is(err, ErrUserNotFound) {
    // Пользователя не существует. Используем UTC.
    tz = time.UTC
  } else {
    return fmt.Errorf("get user %q: %w", id, err)
  }
}
```

</td></tr>
</tbody></table>

### Обрабатывайте неудачные приведения типов

[Приведение типа](https://go.dev/ref/spec#Type_assertions) (type assertion) в форме с одним возвращаемым значением
вызывает панику, если тип неверный. Поэтому всегда используйте идиому «comma ok».

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
t := i.(string)
```

</td><td>

```go
t, ok := i.(string)
if !ok {
  // аккуратно обрабатываем ошибку
}
```

</td></tr>
</tbody></table>

<!-- TODO: There are a few situations where the single assignment form is
fine. -->

### Не паникуйте

Код, работающий в продакшене, должен избегать паник. Паники — основной
источник [каскадных сбоев](https://en.wikipedia.org/wiki/Cascading_failure). Если возникла ошибка, функция должна вернуть её
и позволить вызывающему коду решить, как её обработать.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func run(args []string) {
  if len(args) == 0 {
    panic("an argument is required")
  }
  // ...
}

func main() {
  run(os.Args[1:])
}
```

</td><td>

```go
func run(args []string) error {
  if len(args) == 0 {
    return errors.New("an argument is required")
  }
  // ...
  return nil
}

func main() {
  if err := run(os.Args[1:]); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

</td></tr>
</tbody></table>

Panic/recover — это не стратегия обработки ошибок. Программа должна паниковать
только тогда, когда произошло что-то непоправимое, например разыменование nil.
Исключение — инициализация программы: если при запуске случилось что-то плохое,
из-за чего программу следует прервать, допустима паника.

```go
var _statusTemplate = template.Must(template.New("name").Parse("_statusHTML"))
```

Даже в тестах вместо паники предпочитайте `t.Fatal` или `t.FailNow`, чтобы
тест гарантированно был помечен как проваленный.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  panic("failed to set up test")
}
```

</td><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  t.Fatal("failed to set up test")
}
```

</td></tr>
</tbody></table>

### Используйте go.uber.org/atomic

Атомарные операции пакета [sync/atomic](https://pkg.go.dev/sync/atomic) работают с «сырыми» типами
(`int32`, `int64` и т. д.), поэтому легко забыть использовать атомарную
операцию для чтения или изменения переменной.

[go.uber.org/atomic](https://pkg.go.dev/go.uber.org/atomic) добавляет этим операциям типобезопасность, скрывая
лежащий в основе тип. Кроме того, в нём есть удобный тип `atomic.Bool`.

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
> `sync/atomic` тоже есть типизированные обёртки — [`atomic.Bool`](https://pkg.go.dev/sync/atomic#Bool),
> [`atomic.Int32`](https://pkg.go.dev/sync/atomic#Int32), [`atomic.Int64`](https://pkg.go.dev/sync/atomic#Int64), [`atomic.Pointer[T]`] и другие.
> Пример справа компилируется и с ними без изменений, так что в новом коде
> можно обойтись без внешней зависимости.

[`atomic.Pointer[T]`]: https://pkg.go.dev/sync/atomic#Pointer

### Избегайте изменяемых глобальных переменных

Избегайте изменения глобальных переменных — вместо этого используйте внедрение
зависимостей. Это относится как к указателям на функции, так и к значениям
других видов.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// sign.go

var _timeNow = time.Now

func sign(msg string) string {
  now := _timeNow()
  return signWithTime(msg, now)
}
```

</td><td>

```go
// sign.go

type signer struct {
  now func() time.Time
}

func newSigner() *signer {
  return &signer{
    now: time.Now,
  }
}

func (s *signer) Sign(msg string) string {
  now := s.now()
  return signWithTime(msg, now)
}
```

</td></tr>
<tr><td>

```go
// sign_test.go

func TestSign(t *testing.T) {
  oldTimeNow := _timeNow
  _timeNow = func() time.Time {
    return someFixedTime
  }
  defer func() { _timeNow = oldTimeNow }()

  assert.Equal(t, want, sign(give))
}
```

</td><td>

```go
// sign_test.go

func TestSigner(t *testing.T) {
  s := newSigner()
  s.now = func() time.Time {
    return someFixedTime
  }

  assert.Equal(t, want, s.Sign(give))
}
```

</td></tr>
</tbody></table>

### Не встраивайте типы в публичные структуры

Встроенные типы раскрывают детали реализации, мешают развитию типа
и запутывают документацию.

Допустим, вы реализовали несколько типов списков на основе общего
`AbstractList`. Не встраивайте `AbstractList` в конкретные реализации
списков. Вместо этого вручную напишите в конкретном списке только те методы,
которые будут делегировать вызовы абстрактному списку.

```go
type AbstractList struct {}

// Add добавляет сущность в список.
func (l *AbstractList) Add(e Entity) {
  // ...
}

// Remove удаляет сущность из списка.
func (l *AbstractList) Remove(e Entity) {
  // ...
}
```

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// ConcreteList — список сущностей.
type ConcreteList struct {
  *AbstractList
}
```

</td><td>

```go
// ConcreteList — список сущностей.
type ConcreteList struct {
  list *AbstractList
}

// Add добавляет сущность в список.
func (l *ConcreteList) Add(e Entity) {
  l.list.Add(e)
}

// Remove удаляет сущность из списка.
func (l *ConcreteList) Remove(e Entity) {
  l.list.Remove(e)
}
```

</td></tr>
</tbody></table>

Go допускает [встраивание типов](https://go.dev/doc/effective_go#embedding) как компромисс между наследованием
и композицией. Внешний тип неявно получает копии методов встроенного типа.
По умолчанию эти методы делегируют вызов одноимённому методу встроенного
экземпляра.

Кроме того, структура получает поле с тем же именем, что и тип.
Поэтому если встроенный тип публичный, то и поле публичное.
Чтобы сохранить обратную совместимость, каждая будущая версия внешнего типа
обязана сохранять встроенный тип.

Встраивание типа требуется редко.
Это лишь удобство, избавляющее от написания утомительных методов-делегатов.

Даже встраивание совместимого *интерфейса* AbstractList вместо структуры
дало бы разработчику больше свободы для будущих изменений, но всё равно
раскрыло бы ту деталь, что конкретные списки используют абстрактную реализацию.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// AbstractList — обобщённая реализация
// для различных видов списков сущностей.
type AbstractList interface {
  Add(Entity)
  Remove(Entity)
}

// ConcreteList — список сущностей.
type ConcreteList struct {
  AbstractList
}
```

</td><td>

```go
// ConcreteList — список сущностей.
type ConcreteList struct {
  list AbstractList
}

// Add добавляет сущность в список.
func (l *ConcreteList) Add(e Entity) {
  l.list.Add(e)
}

// Remove удаляет сущность из списка.
func (l *ConcreteList) Remove(e Entity) {
  l.list.Remove(e)
}
```

</td></tr>
</tbody></table>

Будь то встроенная структура или встроенный интерфейс, встроенный тип
ограничивает развитие типа:

- Добавление методов во встроенный интерфейс — несовместимое изменение.
- Удаление методов из встроенной структуры — несовместимое изменение.
- Удаление встроенного типа — несовместимое изменение.
- Замена встроенного типа, даже на альтернативу, реализующую тот же
  интерфейс, — несовместимое изменение.

Писать методы-делегаты утомительно, но эти дополнительные усилия скрывают
детали реализации, оставляют больше возможностей для изменений, а также
избавляют от необходимости переходить по ссылкам, чтобы увидеть полный
интерфейс списка в документации.

### Не используйте имена встроенных идентификаторов

[Спецификация языка](https://go.dev/ref/spec) Go описывает несколько встроенных
[предобъявленных идентификаторов](https://go.dev/ref/spec#Predeclared_identifiers), которые не следует использовать как имена
в программах на Go.

В зависимости от контекста повторное использование этих идентификаторов
в качестве имён либо затенит оригинал в текущей лексической области видимости
(и во всех вложенных), либо сделает код запутанным. В лучшем случае
компилятор пожалуется; в худшем — такой код может содержать скрытые ошибки,
которые сложно найти поиском.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
var error string
// `error` затеняет встроенный тип

// или

func handleErrorMessage(error string) {
    // `error` затеняет встроенный тип
}
```

</td><td>

```go
var errorMessage string
// `error` ссылается на встроенный тип

// или

func handleErrorMessage(msg string) {
    // `error` ссылается на встроенный тип
}
```

</td></tr>
<tr><td>

```go
type Foo struct {
    // Формально эти поля не затеняют
    // встроенные типы, но теперь поиск
    // строк `error` и `string`
    // даёт неоднозначные результаты.
    error  error
    string string
}

func (f Foo) Error() error {
    // `error` и `f.error`
    // визуально похожи
    return f.error
}

func (f Foo) String() string {
    // `string` и `f.string`
    // визуально похожи
    return f.string
}
```

</td><td>

```go
type Foo struct {
    // Поиск строк `error` и `string`
    // теперь однозначен.
    err error
    str string
}

func (f Foo) Error() error {
    return f.err
}

func (f Foo) String() string {
    return f.str
}
```

</td></tr>
</tbody></table>

Обратите внимание: компилятор не выдаёт ошибок при использовании
предобъявленных идентификаторов, но такие инструменты, как `go vet`,
должны корректно указывать на эти и другие случаи затенения.

### Избегайте `init()`

По возможности избегайте `init()`. Если без `init()` не обойтись или его
использование желательно, код должен стремиться:

1. Быть полностью детерминированным независимо от окружения программы и способа
   её запуска.
2. Не зависеть от порядка выполнения и побочных эффектов других функций
   `init()`. Хотя порядок выполнения `init()` хорошо известен, код может
   меняться, и связи между функциями `init()` делают код хрупким
   и подверженным ошибкам.
3. Не обращаться к глобальному состоянию или состоянию окружения и не изменять
   его: информацию о машине, переменные окружения, рабочий каталог, аргументы
   и входные данные программы и т. д.
4. Не выполнять ввод-вывод: ни обращений к файловой системе, ни сетевых,
   ни системных вызовов.

Код, который не удовлетворяет этим требованиям, скорее всего, должен быть
вспомогательной функцией, вызываемой из `main()` (или на другом этапе жизненного
цикла программы), либо частью самой `main()`. В особенности библиотеки,
предназначенные для использования другими программами, должны быть полностью
детерминированными и не заниматься «магией в init».

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type Foo struct {
    // ...
}

var _defaultFoo Foo

func init() {
    _defaultFoo = Foo{
        // ...
    }
}
```

</td><td>

```go
var _defaultFoo = Foo{
    // ...
}

// или, лучше, для тестируемости:

var _defaultFoo = defaultFoo()

func defaultFoo() Foo {
    return Foo{
        // ...
    }
}
```

</td></tr>
<tr><td>

```go
type Config struct {
    // ...
}

var _config Config

func init() {
    // Плохо: зависит от текущего каталога
    cwd, _ := os.Getwd()

    // Плохо: ввод-вывод
    raw, _ := os.ReadFile(
        path.Join(cwd, "config", "config.yaml"),
    )

    yaml.Unmarshal(raw, &_config)
}
```

</td><td>

```go
type Config struct {
    // ...
}

func loadConfig() Config {
    cwd, err := os.Getwd()
    // обрабатываем err

    raw, err := os.ReadFile(
        path.Join(cwd, "config", "config.yaml"),
    )
    // обрабатываем err

    var config Config
    yaml.Unmarshal(raw, &config)

    return config
}
```

</td></tr>
</tbody></table>

С учётом сказанного, `init()` может быть предпочтительным или необходимым,
например, в следующих ситуациях:

- Сложные выражения, которые нельзя записать одним присваиванием.
- Подключаемые хуки, например диалекты `database/sql`, реестры типов
  для кодирования и т. п.
- Оптимизации для [Google Cloud Functions](https://cloud.google.com/functions/docs/bestpractices/tips#use_global_variables_to_reuse_objects_in_future_invocations) и другие формы детерминированных
  предварительных вычислений.

### Завершайте программу в main

Программы на Go используют [`os.Exit`](https://pkg.go.dev/os#Exit) или [`log.Fatal*`](https://pkg.go.dev/log#Fatal) для немедленного
завершения. (Паника — плохой способ завершить программу,
пожалуйста, [не паникуйте](#%D0%BD%D0%B5-%D0%BF%D0%B0%D0%BD%D0%B8%D0%BA%D1%83%D0%B9%D1%82%D0%B5).)

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

#### Завершайте программу один раз

По возможности вызывайте `os.Exit` или `log.Fatal` в `main()` **не более одного
раза**. Если есть несколько сценариев ошибок, прерывающих выполнение программы,
вынесите эту логику в отдельную функцию и возвращайте из неё ошибки.

В результате функция `main()` становится короче, а вся ключевая бизнес-логика
оказывается в отдельной функции, которую можно протестировать.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
package main

func main() {
  args := os.Args[1:]
  if len(args) != 1 {
    log.Fatal("missing file")
  }
  name := args[0]

  f, err := os.Open(name)
  if err != nil {
    log.Fatal(err)
  }
  defer f.Close()

  // Если после этой строки вызвать log.Fatal,
  // f.Close не будет вызван.

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  // ...
}
```

</td><td>

```go
package main

func main() {
  if err := run(); err != nil {
    log.Fatal(err)
  }
}

func run() error {
  args := os.Args[1:]
  if len(args) != 1 {
    return errors.New("missing file")
  }
  name := args[0]

  f, err := os.Open(name)
  if err != nil {
    return err
  }
  defer f.Close()

  b, err := io.ReadAll(f)
  if err != nil {
    return err
  }

  // ...
}
```

</td></tr>
</tbody></table>

В примере выше используется `log.Fatal`, но рекомендация относится и к
`os.Exit`, и к любому библиотечному коду, вызывающему `os.Exit`.

```go
func main() {
  if err := run(); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

Сигнатуру `run()` можно менять под свои нужды.
Например, если программа должна завершаться с определёнными кодами возврата,
`run()` может возвращать код возврата вместо ошибки.
Это также позволяет напрямую проверять такое поведение в модульных тестах.

```go
func main() {
  os.Exit(run(args))
}

func run() (exitCode int) {
  // ...
}
```

В более общем смысле, функция `run()` из этих примеров — не жёсткое
предписание. Её имя, сигнатура и устройство могут быть разными.
Среди прочего, можно:

- принимать неразобранные аргументы командной строки (например, `run(os.Args[1:])`);
- разбирать аргументы командной строки в `main()` и передавать их в `run`;
- использовать собственный тип ошибки, чтобы передать код возврата обратно в `main()`;
- вынести бизнес-логику на другой уровень абстракции, отдельно от `package main`.

Эта рекомендация требует лишь одного: в `main()` должно быть единственное
место, отвечающее за фактическое завершение процесса.

### Используйте теги полей в сериализуемых структурах

Каждое поле структуры, которая сериализуется в JSON, YAML
или другие форматы, поддерживающие именование полей через теги,
должно быть снабжено соответствующим тегом.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type Stock struct {
  Price int
  Name  string
}

bytes, err := json.Marshal(Stock{
  Price: 137,
  Name:  "UBER",
})
```

</td><td>

```go
type Stock struct {
  Price int    `json:"price"`
  Name  string `json:"name"`
  // Name можно безопасно переименовать в Symbol.
}

bytes, err := json.Marshal(Stock{
  Price: 137,
  Name:  "UBER",
})
```

</td></tr>
</tbody></table>

Обоснование:
Сериализованная форма структуры — это контракт между разными системами.
Изменения структуры сериализованной формы, включая имена полей, нарушают
этот контракт. Указание имён полей в тегах делает контракт явным
и защищает от его случайного нарушения при рефакторинге или переименовании полей.

### Не запускайте горутины по принципу «запустил и забыл»

Горутины легковесны, но не бесплатны:
как минимум они расходуют память на стек и процессорное время на планирование.
При типичном использовании горутин эти затраты невелики,
но если запускать их в большом количестве без контроля над временем жизни,
они могут вызвать серьёзные проблемы с производительностью.
Горутины с неуправляемым временем жизни могут вызывать и другие проблемы,
например мешать сборщику мусора освобождать неиспользуемые объекты
и удерживать ресурсы, которые больше не нужны.

Поэтому не допускайте утечек горутин в продакшен-коде.
Используйте [go.uber.org/goleak](https://pkg.go.dev/go.uber.org/goleak),
чтобы проверять утечки горутин в пакетах, которые могут их запускать.

В общем случае для каждой горутины:

- должен быть предсказуемый момент, когда она завершит работу, или
- должен быть способ сообщить горутине, что ей пора остановиться.

В обоих случаях у кода должна быть возможность заблокироваться и дождаться
завершения горутины.

Например:

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
go func() {
  for {
    flush()
    time.Sleep(delay)
  }
}()
```

</td><td>

```go
var (
  stop = make(chan struct{}) // сигнал горутине остановиться
  done = make(chan struct{}) // сигнал нам, что горутина завершилась
)
go func() {
  defer close(done)

  ticker := time.NewTicker(delay)
  defer ticker.Stop()
  for {
    select {
    case <-ticker.C:
      flush()
    case <-stop:
      return
    }
  }
}()

// Где-то в другом месте...
close(stop)  // сигнализируем горутине остановиться
<-done       // и ждём её завершения
```

</td></tr>
<tr><td>

Эту горутину невозможно остановить.
Она будет работать до завершения приложения.

</td><td>

Эту горутину можно остановить с помощью `close(stop)`
и дождаться её завершения с помощью `<-done`.

</td></tr>
</tbody></table>

#### Дожидайтесь завершения горутин

Для любой горутины, запущенной системой,
должен быть способ дождаться её завершения.
Есть два популярных способа это сделать:

- Используйте `sync.WaitGroup`, чтобы дождаться завершения нескольких горутин.
  Этот способ подходит, если нужно дождаться нескольких горутин.

  ```go
  var wg sync.WaitGroup
  for i := 0; i < N; i++ {
    wg.Go(...)
  }

  // Ждём завершения всех горутин:
  wg.Wait()
  ```

- Добавьте ещё один канал `chan struct{}`, который горутина закроет,
  когда закончит работу. Этот способ подходит, если горутина одна.

  ```go
  done := make(chan struct{})
  go func() {
    defer close(done)
    // ...
  }()

  // Ждём завершения горутины:
  <-done
  ```

> **Примечание переводчика.** Метод [`WaitGroup.Go`](https://pkg.go.dev/sync#WaitGroup.Go) появился в Go 1.25.
> В более старых версиях используйте `wg.Add(1)` перед запуском горутины
> и `defer wg.Done()` внутри неё.

#### Не запускайте горутины в `init()`

Функции `init()` не должны запускать горутины.
См. также [Избегайте init()](#%D0%B8%D0%B7%D0%B1%D0%B5%D0%B3%D0%B0%D0%B9%D1%82%D0%B5-init).

Если пакету нужна фоновая горутина,
он должен предоставлять объект, отвечающий за управление её временем жизни.
У этого объекта должен быть метод (`Close`, `Stop`, `Shutdown` и т. п.),
который сигнализирует фоновой горутине остановиться и дожидается её завершения.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func init() {
  go doWork()
}

func doWork() {
  for {
    // ...
  }
}
```

</td><td>

```go
type Worker struct{ /* ... */ }

func NewWorker(...) *Worker {
  w := &Worker{
    stop: make(chan struct{}),
    done: make(chan struct{}),
    // ...
  }
  go w.doWork()
  return w
}

func (w *Worker) doWork() {
  defer close(w.done)
  for {
    // ...
    case <-w.stop:
      return
  }
}

// Shutdown сообщает воркеру, что нужно остановиться,
// и ждёт, пока он завершит работу.
func (w *Worker) Shutdown() {
  close(w.stop)
  <-w.done
}
```

</td></tr>
<tr><td>

Безусловно запускает фоновую горутину, когда пользователь импортирует пакет.
У пользователя нет ни контроля над горутиной, ни способа её остановить.

</td><td>

Запускает воркер, только если пользователь его запросил.
Предоставляет способ остановить воркер, чтобы пользователь мог освободить
занимаемые им ресурсы.

Обратите внимание: если воркер управляет несколькими горутинами,
следует использовать `WaitGroup`.
См. [Дожидайтесь завершения горутин](#%D0%B4%D0%BE%D0%B6%D0%B8%D0%B4%D0%B0%D0%B9%D1%82%D0%B5%D1%81%D1%8C-%D0%B7%D0%B0%D0%B2%D0%B5%D1%80%D1%88%D0%B5%D0%BD%D0%B8%D1%8F-%D0%B3%D0%BE%D1%80%D1%83%D1%82%D0%B8%D0%BD).

</td></tr>
</tbody></table>

## Производительность

Рекомендации, касающиеся производительности, применимы только к горячему пути
(hot path) — наиболее часто выполняемому коду.

### Предпочитайте strconv вместо fmt

При преобразовании примитивов в строки и обратно `strconv` работает быстрее,
чем `fmt`.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
for i := 0; i < b.N; i++ {
  s := fmt.Sprint(rand.Int())
}
```

</td><td>

```go
for i := 0; i < b.N; i++ {
  s := strconv.Itoa(rand.Int())
}
```

</td></tr>
<tr><td>

```plain
BenchmarkFmtSprint-4    143 ns/op    2 allocs/op
```

</td><td>

```plain
BenchmarkStrconv-4    64.2 ns/op    1 allocs/op
```

</td></tr>
</tbody></table>

### Избегайте повторных преобразований строки в срез байтов

Не создавайте срез байтов из одной и той же строки многократно. Вместо этого
выполните преобразование один раз и сохраните результат.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
for i := 0; i < b.N; i++ {
  w.Write([]byte("Hello world"))
}
```

</td><td>

```go
data := []byte("Hello world")
for i := 0; i < b.N; i++ {
  w.Write(data)
}
```

</td></tr>
<tr><td>

```plain
BenchmarkBad-4   50000000   22.2 ns/op
```

</td><td>

```plain
BenchmarkGood-4  500000000   3.25 ns/op
```

</td></tr>
</tbody></table>

### Указывайте ёмкость контейнеров

По возможности указывайте ёмкость (capacity) контейнера, чтобы выделить
под него память заранее. Это сводит к минимуму последующие выделения памяти
(на копирование и изменение размера контейнера) при добавлении элементов.

#### Подсказка ёмкости для мап

По возможности передавайте подсказку ёмкости при инициализации мапы
с помощью `make()`.

```go
make(map[T1]T2, hint)
```

Подсказка ёмкости в `make()` позволяет сразу подобрать подходящий размер мапы
при инициализации, что уменьшает необходимость в её росте и выделениях памяти
при добавлении элементов.

Учтите, что, в отличие от срезов, подсказка ёмкости мапы не гарантирует
полного предварительного выделения памяти, а лишь используется для оценки
количества нужных бакетов хеш-таблицы. Поэтому при добавлении элементов
в мапу выделения памяти всё равно возможны, даже в пределах указанной ёмкости.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
files, _ := os.ReadDir("./files")

m := make(map[string]os.DirEntry)
for _, f := range files {
    m[f.Name()] = f
}
```

</td><td>

```go

files, _ := os.ReadDir("./files")

m := make(map[string]os.DirEntry, len(files))
for _, f := range files {
    m[f.Name()] = f
}
```

</td></tr>
<tr><td>

`m` создаётся без подсказки размера; мапа будет расширяться
динамически, и по мере роста произойдёт несколько выделений памяти.

</td><td>

`m` создаётся с подсказкой размера; выделений памяти
при присваивании может быть меньше.

</td></tr>
</tbody></table>

#### Ёмкость срезов

По возможности указывайте ёмкость при инициализации срезов с помощью `make()`,
особенно если затем в них добавляются элементы.

```go
make([]T, length, capacity)
```

В отличие от мап, ёмкость среза — не подсказка: компилятор выделит
достаточно памяти под ёмкость, переданную в `make()`. Это значит, что
последующие операции `append()` не будут выделять память (пока длина среза
не сравняется с ёмкостью — после этого для добавления новых элементов
потребуется изменить размер).

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
for n := 0; n < b.N; n++ {
  data := make([]int, 0)
  for k := 0; k < size; k++{
    data = append(data, k)
  }
}
```

</td><td>

```go
for n := 0; n < b.N; n++ {
  data := make([]int, 0, size)
  for k := 0; k < size; k++{
    data = append(data, k)
  }
}
```

</td></tr>
<tr><td>

```plain
BenchmarkBad-4    100000000    2.48s
```

</td><td>

```plain
BenchmarkGood-4   100000000    0.21s
```

</td></tr>
</tbody></table>

## Стиль

### Избегайте слишком длинных строк

Избегайте строк кода, из-за которых читателю приходится прокручивать текст
по горизонтали или слишком сильно поворачивать голову.

Мы рекомендуем мягкое ограничение длины строки в **99 символов**.
Авторам следует стремиться переносить строки до достижения этого предела,
но это не жёсткое ограничение.
Код может его превышать.

### Будьте последовательны

Некоторые рекомендации из этого документа можно оценить объективно;
другие зависят от ситуации, контекста или субъективны.

Но прежде всего — **будьте последовательны**.

Последовательный код проще сопровождать, о нём легче рассуждать, он требует
меньше когнитивных усилий, и его проще переносить или обновлять, когда появляются
новые соглашения или исправляются целые классы ошибок.

И наоборот, несколько разрозненных или противоречащих друг другу стилей в одной
кодовой базе приводят к издержкам на сопровождение, неопределённости
и когнитивному диссонансу — всё это напрямую снижает скорость разработки,
делает ревью кода мучительным и порождает ошибки.

Применяя эти рекомендации к кодовой базе, изменения стоит вносить на уровне
пакета (или выше): применение на уровне части пакета нарушает сказанное выше,
поскольку вносит несколько стилей в один и тот же код.

### Группируйте похожие объявления

Go поддерживает группировку похожих объявлений.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
import "a"
import "b"
```

</td><td>

```go
import (
  "a"
  "b"
)
```

</td></tr>
</tbody></table>

Это относится также к константам, переменным и объявлениям типов.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go

const a = 1
const b = 2



var a = 1
var b = 2



type Area float64
type Volume float64
```

</td><td>

```go
const (
  a = 1
  b = 2
)

var (
  a = 1
  b = 2
)

type (
  Area float64
  Volume float64
)
```

</td></tr>
</tbody></table>

Группируйте только связанные объявления. Не группируйте объявления,
не связанные друг с другом.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
  EnvVar = "MY_ENV"
)
```

</td><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)

const EnvVar = "MY_ENV"
```

</td></tr>
</tbody></table>

Группы можно использовать где угодно. Например, внутри функций.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func f() string {
  red := color.New(0xff0000)
  green := color.New(0x00ff00)
  blue := color.New(0x0000ff)

  // ...
}
```

</td><td>

```go
func f() string {
  var (
    red   = color.New(0xff0000)
    green = color.New(0x00ff00)
    blue  = color.New(0x0000ff)
  )

  // ...
}
```

</td></tr>
</tbody></table>

Исключение: объявления переменных, особенно внутри функций, следует
группировать, если они расположены рядом с другими переменными. Делайте так
для переменных, объявленных вместе, даже если они не связаны между собой.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func (c *client) request() {
  caller := c.name
  format := "json"
  timeout := 5*time.Second
  var err error

  // ...
}
```

</td><td>

```go
func (c *client) request() {
  var (
    caller  = c.name
    format  = "json"
    timeout = 5*time.Second
    err error
  )

  // ...
}
```

</td></tr>
</tbody></table>

### Порядок групп импорта

Импорты должны быть разделены на две группы:

- стандартная библиотека;
- всё остальное.

Именно такую группировку по умолчанию применяет goimports.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
import (
  "fmt"
  "os"
  "go.uber.org/atomic"
  "golang.org/x/sync/errgroup"
)
```

</td><td>

```go
import (
  "fmt"
  "os"

  "go.uber.org/atomic"
  "golang.org/x/sync/errgroup"
)
```

</td></tr>
</tbody></table>

### Имена пакетов

Выбирайте для пакета имя, которое:

- Написано целиком в нижнем регистре. Без заглавных букв и подчёркиваний.
- В большинстве мест использования не требует переименования
  с помощью именованного импорта.
- Короткое и ёмкое. Помните, что имя пакета полностью указывается при каждом
  обращении к нему.
- Стоит в единственном числе. Например, `net/url`, а не `net/urls`.
- Не «common», «util», «shared» или «lib». Это плохие, неинформативные имена.

См. также [Package Names](https://go.dev/blog/package-names) и [Style guideline for Go packages](https://rakyll.org/style-packages/).

### Имена функций

Мы следуем принятому в сообществе Go соглашению об использовании
[MixedCaps в именах функций](https://go.dev/doc/effective_go#mixed-caps). Исключение составляют тестовые функции:
их имена могут содержать подчёркивания для группировки связанных тестовых
случаев, например `TestMyFunction_WhatIsBeingTested`.

### Псевдонимы импортов

Псевдоним импорта обязателен, если имя пакета не совпадает с последним
элементом пути импорта.

```go
import (
  "net/http"

  client "example.com/client-go"
  trace "example.com/trace/v2"
)
```

Во всех остальных случаях псевдонимов импортов следует избегать, если только
между импортами нет прямого конфликта имён.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
import (
  "fmt"
  "os"
  runtimetrace "runtime/trace"

  nettrace "golang.net/x/trace"
)
```

</td><td>

```go
import (
  "fmt"
  "os"
  "runtime/trace"

  nettrace "golang.net/x/trace"
)
```

</td></tr>
</tbody></table>

### Группировка и порядок функций

- Функции должны быть упорядочены примерно в порядке их вызова.
- Функции в файле должны быть сгруппированы по получателю.

Следовательно, экспортируемые функции должны идти в файле первыми, после
определений `struct`, `const` и `var`.

Функция `newXYZ()`/`NewXYZ()` может идти сразу после определения типа,
но перед остальными методами этого получателя.

Поскольку функции сгруппированы по получателю, простые вспомогательные функции
должны располагаться ближе к концу файла.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func (s *something) Cost() {
  return calcCost(s.weights)
}

type something struct{ ... }

func calcCost(n []int) int {...}

func (s *something) Stop() {...}

func newSomething() *something {
    return &something{}
}
```

</td><td>

```go
type something struct{ ... }

func newSomething() *something {
    return &something{}
}

func (s *something) Cost() {
  return calcCost(s.weights)
}

func (s *something) Stop() {...}

func calcCost(n []int) int {...}
```

</td></tr>
</tbody></table>

### Уменьшайте вложенность

По возможности уменьшайте вложенность кода: сначала обрабатывайте ошибки
и особые случаи, сразу возвращаясь из функции или переходя к следующей итерации
цикла. Сокращайте объём кода, вложенного на несколько уровней.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
for _, v := range data {
  if v.F1 == 1 {
    v = process(v)
    if err := v.Call(); err == nil {
      v.Send()
    } else {
      return err
    }
  } else {
    log.Printf("Invalid v: %v", v)
  }
}
```

</td><td>

```go
for _, v := range data {
  if v.F1 != 1 {
    log.Printf("Invalid v: %v", v)
    continue
  }

  v = process(v)
  if err := v.Call(); err != nil {
    return err
  }
  v.Send()
}
```

</td></tr>
</tbody></table>

### Лишний else

Если переменной присваивается значение в обеих ветках if, это можно заменить
одним if.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
var a int
if b {
  a = 100
} else {
  a = 10
}
```

</td><td>

```go
a := 10
if b {
  a = 100
}
```

</td></tr>
</tbody></table>

### Объявление переменных верхнего уровня

На верхнем уровне используйте стандартное ключевое слово `var`. Не указывайте
тип, если он совпадает с типом выражения.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
var _s string = F()

func F() string { return "A" }
```

</td><td>

```go
var _s = F()
// F уже объявляет, что возвращает string,
// поэтому повторно указывать тип не нужно.

func F() string { return "A" }
```

</td></tr>
</tbody></table>

Указывайте тип, если тип выражения не совпадает с нужным типом в точности.

```go
type myError struct{}

func (myError) Error() string { return "error" }

func F() myError { return myError{} }

var _e error = F()
// F возвращает объект типа myError, а нам нужен error.
```

### Используйте префикс _ для неэкспортируемых глобальных переменных

Добавляйте префикс `_` к неэкспортируемым `var` и `const` верхнего уровня,
чтобы при их использовании было ясно, что это глобальные символы.

Обоснование: переменные и константы верхнего уровня видны во всём пакете.
Если у них общие имена, легко случайно использовать не то значение
в другом файле.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
// foo.go

const (
  defaultPort = 8080
  defaultUser = "user"
)

// bar.go

func Bar() {
  defaultPort := 9090
  ...
  fmt.Println("Default port", defaultPort)

  // Если удалить первую строку Bar(),
  // ошибки компиляции не будет.
}
```

</td><td>

```go
// foo.go

const (
  _defaultPort = 8080
  _defaultUser = "user"
)
```

</td></tr>
</tbody></table>

**Исключение**: неэкспортируемые значения ошибок могут использовать префикс `err`
без подчёркивания. См. [Именование ошибок](#%D0%B8%D0%BC%D0%B5%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BE%D1%88%D0%B8%D0%B1%D0%BE%D0%BA).

### Встраивание в структуры

Встроенные типы должны располагаться в начале списка полей структуры
и отделяться от обычных полей пустой строкой.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type Client struct {
  version int
  http.Client
}
```

</td><td>

```go
type Client struct {
  http.Client

  version int
}
```

</td></tr>
</tbody></table>

Встраивание должно давать ощутимую пользу, например добавлять или расширять
функциональность семантически уместным образом. При этом оно не должно иметь
никаких негативных последствий для пользователей (см. также:
[Не встраивайте типы в публичные структуры](#%D0%BD%D0%B5-%D0%B2%D1%81%D1%82%D1%80%D0%B0%D0%B8%D0%B2%D0%B0%D0%B9%D1%82%D0%B5-%D1%82%D0%B8%D0%BF%D1%8B-%D0%B2-%D0%BF%D1%83%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5-%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D1%8B)).

Исключение: мьютексы не следует встраивать даже в неэкспортируемые типы.
См. также: [Нулевое значение мьютекса валидно](#%D0%BD%D1%83%D0%BB%D0%B5%D0%B2%D0%BE%D0%B5-%D0%B7%D0%BD%D0%B0%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-%D0%BC%D1%8C%D1%8E%D1%82%D0%B5%D0%BA%D1%81%D0%B0-%D0%B2%D0%B0%D0%BB%D0%B8%D0%B4%D0%BD%D0%BE).

Встраивание **не должно**:

- Быть чисто косметическим или служить только для удобства.
- Усложнять создание или использование внешнего типа.
- Влиять на нулевое значение внешнего типа. Если у внешнего типа есть полезное
  нулевое значение, оно должно оставаться полезным и после встраивания
  внутреннего типа.
- Раскрывать у внешнего типа не относящиеся к нему функции или поля
  как побочный эффект встраивания внутреннего типа.
- Раскрывать неэкспортируемые типы.
- Влиять на семантику копирования внешнего типа.
- Менять API или семантику внешнего типа.
- Встраивать неканоническую форму внутреннего типа.
- Раскрывать детали реализации внешнего типа.
- Позволять пользователям наблюдать за внутренним устройством типа
  или управлять им.
- Менять общее поведение внутренних функций за счёт обёртывания так,
  что это обоснованно удивит пользователей.

Проще говоря, встраивайте осознанно и намеренно. Хорошая проверка:
«добавили бы мы все эти экспортируемые методы и поля внутреннего типа
напрямую во внешний тип?» Если ответ «некоторые» или «нет», не встраивайте
внутренний тип — используйте поле.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
type A struct {
    // Плохо: теперь доступны A.Lock()
    //        и A.Unlock(), они не дают
    //        никакой пользы и позволяют
    //        пользователям управлять
    //        внутренним устройством A.
    sync.Mutex
}
```

</td><td>

```go
type countingWriteCloser struct {
    // Хорошо: Write() предоставляется
    //         на внешнем уровне с конкретной
    //         целью и делегирует работу
    //         методу Write() внутреннего типа.
    io.WriteCloser

    count int
}

func (w *countingWriteCloser) Write(bs []byte) (int, error) {
    w.count += len(bs)
    return w.WriteCloser.Write(bs)
}
```

</td></tr>
<tr><td>

```go
type Book struct {
    // Плохо: указатель делает нулевое
    // значение бесполезным
    io.ReadWriter

    // другие поля
}

// позже

var b Book
b.Read(...)  // panic: nil pointer
b.String()   // panic: nil pointer
b.Write(...) // panic: nil pointer
```

</td><td>

```go
type Book struct {
    // Хорошо: у типа полезное
    // нулевое значение
    bytes.Buffer

    // другие поля
}

// позже

var b Book
b.Read(...)  // ok
b.String()   // ok
b.Write(...) // ok
```

</td></tr>
<tr><td>

```go
type Client struct {
    sync.Mutex
    sync.WaitGroup
    bytes.Buffer
    url.URL
}
```

</td><td>

```go
type Client struct {
    mtx sync.Mutex
    wg  sync.WaitGroup
    buf bytes.Buffer
    url url.URL
}
```

</td></tr>
</tbody></table>

### Объявление локальных переменных

Если переменной явно присваивается значение, используйте короткое объявление
переменной (`:=`).

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
var s = "foo"
```

</td><td>

```go
s := "foo"
```

</td></tr>
</tbody></table>

Однако бывают случаи, когда значение по умолчанию понятнее при использовании
ключевого слова `var`. Например, при [объявлении пустых срезов](https://go.dev/wiki/CodeReviewComments#declaring-empty-slices).

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
func f(list []int) {
  filtered := []int{}
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td><td>

```go
func f(list []int) {
  var filtered []int
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td></tr>
</tbody></table>

### nil — валидный срез

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

### Сужайте область видимости переменных

По возможности сужайте область видимости переменных и констант. Не делайте
этого, если это противоречит правилу [Уменьшайте вложенность](#%D1%83%D0%BC%D0%B5%D0%BD%D1%8C%D1%88%D0%B0%D0%B9%D1%82%D0%B5-%D0%B2%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%BD%D0%BE%D1%81%D1%82%D1%8C).

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
err := os.WriteFile(name, data, 0644)
if err != nil {
 return err
}
```

</td><td>

```go
if err := os.WriteFile(name, data, 0644); err != nil {
 return err
}
```

</td></tr>
</tbody></table>

Если результат вызова функции нужен за пределами if, не пытайтесь сузить
область видимости.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
if data, err := os.ReadFile(name); err == nil {
  err = cfg.Decode(data)
  if err != nil {
    return err
  }

  fmt.Println(cfg)
  return nil
} else {
  return err
}
```

</td><td>

```go
data, err := os.ReadFile(name)
if err != nil {
   return err
}

if err := cfg.Decode(data); err != nil {
  return err
}

fmt.Println(cfg)
return nil
```

</td></tr>
</tbody></table>

Константы не обязаны быть глобальными, если только они не используются
в нескольких функциях или файлах и не являются частью внешнего контракта пакета.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
const (
  _defaultPort = 8080
  _defaultUser = "user"
)

func Bar() {
  fmt.Println("Default port", _defaultPort)
}
```

</td><td>

```go
func Bar() {
  const (
    defaultPort = 8080
    defaultUser = "user"
  )
  fmt.Println("Default port", defaultPort)
}
```

</td></tr>
</tbody></table>

### Избегайте «голых» параметров

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

### Используйте сырые строковые литералы, чтобы избежать экранирования

Go поддерживает [сырые строковые литералы](https://go.dev/ref/spec#raw_string_lit)
(raw string literals), которые могут занимать несколько строк и содержать кавычки.
Используйте их, чтобы избежать строк с ручным экранированием, которые гораздо
труднее читать.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
wantError := "unknown name:\"test\""
```

</td><td>

```go
wantError := `unknown name:"test"`
```

</td></tr>
</tbody></table>

### Инициализация структур

#### Используйте имена полей при инициализации структур

При инициализации структур почти всегда следует указывать имена полей.
Теперь этого требует [`go vet`](https://pkg.go.dev/cmd/vet).

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
k := User{"John", "Doe", true}
```

</td><td>

```go
k := User{
    FirstName: "John",
    LastName: "Doe",
    Admin: true,
}
```

</td></tr>
</tbody></table>

Исключение: в тестовых таблицах имена полей *можно* опускать,
если полей 3 или меньше.

```go
tests := []struct{
  op Operation
  want string
}{
  {Add, "add"},
  {Subtract, "subtract"},
}
```

#### Опускайте поля с нулевыми значениями

При инициализации структур с именами полей опускайте поля с нулевыми
значениями, если только они не несут осмысленного контекста. В остальных
случаях позвольте Go автоматически присвоить им нулевые значения.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
  MiddleName: "",
  Admin: false,
}
```

</td><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
}
```

</td></tr>
</tbody></table>

Это уменьшает шум для читателя за счёт отказа от значений, которые в данном
контексте и так используются по умолчанию. Указываются только осмысленные
значения.

Указывайте нулевые значения, если имена полей дают осмысленный контекст.
Например, тестовым случаям в [тестовых таблицах](#%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%87%D0%BD%D1%8B%D0%B5-%D1%82%D0%B5%D1%81%D1%82%D1%8B) имена полей
могут быть полезны, даже когда значения нулевые.

```go
tests := []struct{
  give string
  want int
}{
  {give: "0", want: 0},
  // ...
}
```

#### Используйте `var` для структур с нулевым значением

Если при объявлении структуры все её поля опущены, используйте форму
объявления с `var`.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{}
```

</td><td>

```go
var user User
```

</td></tr>
</tbody></table>

Так структуры с нулевым значением отличаются от структур с ненулевыми полями —
аналогично разнице, принятой для [инициализации мап](#%D0%B8%D0%BD%D0%B8%D1%86%D0%B8%D0%B0%D0%BB%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F-%D0%BC%D0%B0%D0%BF), — и это
соответствует тому, как мы предпочитаем [объявлять пустые срезы](https://go.dev/wiki/CodeReviewComments#declaring-empty-slices).

#### Инициализация ссылок на структуры

При инициализации ссылок на структуры используйте `&T{}` вместо `new(T)`,
чтобы это было единообразно с инициализацией структур.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
sval := T{Name: "foo"}

// неединообразно
sptr := new(T)
sptr.Name = "bar"
```

</td><td>

```go
sval := T{Name: "foo"}

sptr := &T{Name: "bar"}
```

</td></tr>
</tbody></table>

### Инициализация мап

Для пустых мап и мап, заполняемых программно, предпочитайте `make(..)`.
Так инициализация мапы визуально отличается от объявления, и в дальнейшем
к ней легко добавить подсказку размера, если он станет известен.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
var (
  // m1 безопасна для чтения и записи;
  // m2 вызовет панику при записи.
  m1 = map[T1]T2{}
  m2 map[T1]T2
)
```

</td><td>

```go
var (
  // m1 безопасна для чтения и записи;
  // m2 вызовет панику при записи.
  m1 = make(map[T1]T2)
  m2 map[T1]T2
)
```

</td></tr>
<tr><td>

Объявление и инициализация визуально похожи.

</td><td>

Объявление и инициализация визуально различаются.

</td></tr>
</tbody></table>

По возможности указывайте подсказку ёмкости при инициализации мап
с помощью `make()`. Подробнее см. раздел
[Подсказка ёмкости для мап](#%D0%BF%D0%BE%D0%B4%D1%81%D0%BA%D0%B0%D0%B7%D0%BA%D0%B0-%D1%91%D0%BC%D0%BA%D0%BE%D1%81%D1%82%D0%B8-%D0%B4%D0%BB%D1%8F-%D0%BC%D0%B0%D0%BF).

С другой стороны, если мапа содержит фиксированный набор элементов,
инициализируйте её литералом.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
m := make(map[T1]T2, 3)
m[k1] = v1
m[k2] = v2
m[k3] = v3
```

</td><td>

```go
m := map[T1]T2{
  k1: v1,
  k2: v2,
  k3: v3,
}
```

</td></tr>
</tbody></table>

Простое практическое правило: используйте литералы мап, если при инициализации
добавляется фиксированный набор элементов, а в остальных случаях — `make`
(с подсказкой размера, если он известен).

### Строки формата вне Printf

Если вы объявляете строки формата для функций в стиле `Printf` не в виде
строкового литерала прямо в вызове, делайте их константами (`const`).

Это помогает `go vet` выполнять статический анализ строки формата.

<table>
<thead><tr><th>Плохо</th><th>Хорошо</th></tr></thead>
<tbody>
<tr><td>

```go
msg := "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td><td>

```go
const msg = "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td></tr>
</tbody></table>

### Именование функций в стиле Printf

Объявляя функцию в стиле `Printf`, убедитесь, что `go vet` сможет её
распознать и проверить строку формата.

Это значит, что по возможности следует использовать предопределённые имена
функций в стиле `Printf`. `go vet` проверяет их по умолчанию. Подробнее см.
[Printf family](https://pkg.go.dev/cmd/vet#hdr-Printf_family).

Если использовать предопределённые имена нельзя, заканчивайте выбранное имя
буквой f: `Wrapf`, а не `Wrap`. `go vet` можно попросить проверять
определённые имена в стиле `Printf`, но они должны заканчиваться на f.

```shell
go vet -printfuncs=wrapf,statusf
```

См. также [go vet: Printf family check](https://kuzminva.wordpress.com/2017/11/07/go-vet-printf-family-check/).

## Паттерны

### Табличные тесты

Табличные тесты с [подтестами](https://go.dev/blog/subtests) — полезный паттерн, позволяющий избежать
дублирования кода, когда основная логика теста повторяется.

Если тестируемую систему нужно проверить в *нескольких условиях*, когда
меняются определённые части входных и выходных данных, следует использовать
табличный тест, чтобы уменьшить избыточность и улучшить читаемость.

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

#### Избегайте лишней сложности в табличных тестах

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

#### Параллельные тесты

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

### Функциональные опции

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

- [Self-referential functions and the design of options](https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html)
- [Functional options for friendly APIs](https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis)

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->

## Линтинг

Важнее любого «одобренного» набора линтеров — применять линтеры единообразно
во всей кодовой базе.

Как минимум мы рекомендуем следующие линтеры: на наш взгляд, они помогают
выявлять самые распространённые проблемы и задают высокую планку качества
кода, не будучи при этом излишне строгими:

- [errcheck](https://github.com/kisielk/errcheck) — чтобы убедиться, что ошибки обрабатываются;
- [goimports](https://pkg.go.dev/golang.org/x/tools/cmd/goimports) — для форматирования кода и управления импортами;
- [revive](https://github.com/mgechev/revive) — чтобы указывать на распространённые стилистические ошибки;
- [govet](https://pkg.go.dev/cmd/vet) — для анализа кода на распространённые ошибки;
- [staticcheck](https://staticcheck.dev) — для различных проверок статического анализа.

  > **Примечание**: [revive](https://github.com/mgechev/revive) — современный и более быстрый преемник
  > устаревшего [golint](https://github.com/golang/lint).

### Инструменты запуска линтеров

В качестве основного инструмента для запуска линтеров в коде на Go мы
рекомендуем [golangci-lint](https://github.com/golangci/golangci-lint) — в основном из-за его производительности
на крупных кодовых базах и возможности настраивать и запускать сразу множество
канонических линтеров. В оригинальном репозитории есть пример конфигурационного
файла [.golangci.yml](https://github.com/uber-go/guide/blob/master/.golangci.yml) с рекомендуемыми линтерами и настройками.

В golangci-lint доступно [множество линтеров](https://golangci-lint.run/usage/linters/). Перечисленные выше линтеры
рекомендуются как базовый набор, и мы призываем команды добавлять любые другие
линтеры, которые имеют смысл для их проектов.
