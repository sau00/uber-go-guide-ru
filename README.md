Это русский перевод [Uber Go Style Guide](https://github.com/uber-go/guide) —
руководства, в котором описаны паттерны и соглашения, используемые в коде
на Go в Uber.

## Руководство по стилю

Само руководство — в файле [style.md](style.md).

Перевод синхронизирован с оригиналом по состоянию на апрель 2026 года
(коммит [`1d60a91`](https://github.com/uber-go/guide/commit/1d60a91aa5e87d443002e23c21903c49489dbde5)).

## Как устроен репозиторий

Как и в оригинале, `style.md` собирается из файлов каталога [src](src)
с помощью [stitchmd](https://github.com/abhinav/stitchmd). Правьте файлы в `src`,
а затем пересоберите `style.md`:

```bash
make
```

Подробнее — в [CONTRIBUTING.md](CONTRIBUTING.md).

## Переводы

Известные нам переводы этого руководства, сделанные сообществом Go:

- **中文翻译** (Chinese): [xxjwxc/uber_go_guide_cn](https://github.com/xxjwxc/uber_go_guide_cn)
- **繁體中文** (Traditional Chinese): [ianchen0119/uber_go_guide_tw](https://github.com/ianchen0119/uber_go_guide_tw)
- **한국어 번역** (Korean): [TangoEnSkai/uber-go-style-guide-kr](https://github.com/TangoEnSkai/uber-go-style-guide-kr)
- **日本語訳** (Japanese): [knsh14/uber-style-guide-ja](https://github.com/knsh14/uber-style-guide-ja)
- **Traducción al Español** (Spanish): [friendsofgo/uber-go-guide-es](https://github.com/friendsofgo/uber-go-guide-es)
- **แปลภาษาไทย** (Thai): [pallat/uber-go-style-guide-th](https://github.com/pallat/uber-go-style-guide-th)
- **Tradução em português** (Portuguese): [lucassscaravelli/uber-go-guide-pt](https://github.com/lucassscaravelli/uber-go-guide-pt)
- **Tradução em português** (Portuguese BR): [alcir-junior-caju/uber-go-style-guide-pt-br](https://github.com/alcir-junior-caju/uber-go-style-guide-pt-br)
- **Tłumaczenie polskie** (Polish): [DamianSkrzypczak/uber-go-guide-pl](https://github.com/DamianSkrzypczak/uber-go-guide-pl)
- **Русский перевод** (Russian): [sau00/uber-go-guide-ru](https://github.com/sau00/uber-go-guide-ru), [alekarah/uber-go-guide-ru](https://github.com/alekarah/uber-go-guide-ru)
- **Français** (French): [rm3l/uber-go-style-guide-fr](https://github.com/rm3l/uber-go-style-guide-fr)
- **Türkçe** (Turkish): [ksckaan1/uber-go-style-guide-tr](https://github.com/ksckaan1/uber-go-style-guide-tr)
- **Український переклад** (Ukrainian): [vorobeyme/uber-go-style-guide-uk](https://github.com/vorobeyme/uber-go-style-guide-uk)
- **ترجمه فارسی** (Persian): [jamalkaksouri/uber-go-guide-ir](https://github.com/jamalkaksouri/uber-go-guide-ir)
- **Tiếng việt** (Vietnamese): [nc-minh/uber-go-guide-vi](https://github.com/nc-minh/uber-go-guide-vi)
- **العربية** (Arabic): [anqorithm/uber-go-guide-ar](https://github.com/anqorithm/uber-go-guide-ar)
- **Bahasa Indonesia** (Indonesian): [stanleydv12/uber-go-guide-id](https://github.com/stanleydv12/uber-go-guide-id)

Если у вас есть перевод, присылайте PR в
[оригинальный репозиторий](https://github.com/uber-go/guide), чтобы добавить его в список.
