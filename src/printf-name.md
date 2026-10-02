# Именование функций в стиле Printf

Объявляя функцию в стиле `Printf`, убедитесь, что `go vet` сможет её
распознать и проверить строку формата.

Это значит, что по возможности следует использовать предопределённые имена
функций в стиле `Printf`. `go vet` проверяет их по умолчанию. Подробнее см.
[Printf family].

  [Printf family]: https://pkg.go.dev/cmd/vet#hdr-Printf_family

Если использовать предопределённые имена нельзя, заканчивайте выбранное имя
буквой f: `Wrapf`, а не `Wrap`. `go vet` можно попросить проверять
определённые имена в стиле `Printf`, но они должны заканчиваться на f.

```shell
go vet -printfuncs=wrapf,statusf
```

См. также [go vet: Printf family check].

  [go vet: Printf family check]: https://kuzminva.wordpress.com/2017/11/07/go-vet-printf-family-check/
