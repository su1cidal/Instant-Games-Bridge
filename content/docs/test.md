---
title: TEST
type: docs
weight: 5
---

# test

{{< colour red  >}}false{{< /colour >}}
{{< colour green  >}}true{{< /colour >}}

{{< colour red  >}}*false*{{< /colour >}}
{{< colour green  >}}*true*{{< /colour >}}

{{< colour red  >}}**false**{{< /colour >}}
{{< colour green  >}}**true**{{< /colour >}}
~~~
bridge.storage.isAvailable(storageType)
~~~

| Платформа         | local_storage | platform_internal                               |
|-------------------|---------------|-------------------------------------------------|
| VK                | {{< colour green  >}}**true**{{< /colour >}}        | {{< colour green  >}}**true**{{< /colour >}}                                            |
| Yandex            | {{< colour green  >}}**true**{{< /colour >}}        | Если игрок авторизован — {{< colour green  >}}**true**{{< /colour >}}, если нет — {{< colour red  >}}**false**{{< /colour >}} |
| Crazy Games       | {{< colour green  >}}**true**{{< /colour >}}        | {{< colour red  >}}**false**{{< /colour >}}                                          |
| Absolute Games    | {{< colour green  >}}**true**{{< /colour >}}        | Если игрок авторизован — {{< colour green  >}}true{{< /colour >}}, если нет — {{< colour red  >}}**false**{{< /colour >}} |
| Game Distribution | {{< colour green  >}}**true**{{< /colour >}}        | {{< colour red  >}}**false**{{< /colour >}}                                          |
| Mock              | {{< colour green  >}}**true**{{< /colour >}}        | {{< colour red  >}}**false**{{< /colour >}}          |

| Платформа         | true |false|
|-------------------|---------------|--|
| 1                | {{< colourtext green  >}}**true**{{< /colour >}}|{{< colourtext red  >}}**false**{{< /colour >}}|
| 2                | {{< colourtext green  >}}true{{< /colour >}}|{{< colourtext red  >}}false{{< /colour >}}|
| 3                | {{< colour green  >}}**true**{{< /colour >}}|{{< colour red  >}}**false**{{< /colour >}}|
| 4                | {{< colour green  >}}true{{< /colour >}}|{{< colour red  >}}**false**{{< /colour >}}|
| 5                | `true`|`false`|
| 6                | true|false|
| Платформа         | local_storage | platform_internal                               |
|-------------------|---------------|-------------------------------------------------|
| VK                | `true`        | `true`                                            |
| Yandex            | `true`        | Если игрок авторизован — `true`, если нет — `false` |
| Crazy Games       | `true`        | `false`                                           |
| Absolute Games    | `true`        | Если игрок авторизован — `true`, если нет — `false` |
| Game Distribution | `true`        | `false`                                           |
| Mock              | `true`        | `false`                                           |
## Подключение

Скачайте файл instant-games-bridge.js со страницы релизов на гитхаб, добавьте в свой проект и подключите в index.html:

## Возможности

- Сохранение и загрузка прогресса игрока
- Монетизация: Banner, Interstitial, Rewarded
- Социальные функции (поделиться, пригласить друга, добавить в избранное, etc.)
- Лидерборды
- Информация о языке, девайсе
- И другое


## Полезные ссылки

- Роадмап: [trello.com](https://trello.com/b/NjF29vTW/instant-games-bridge-roadmap)
- Телеграм-чат: [@instant_games_bridge](https://t.me/instant_games_bridge)
- Телеграм-чат про разработку игр под разные веб-платформы: [@WebGamedev](https://t.me/WebGamedev)
- Поддержать проект: [cloudtips.ru](https://pay.cloudtips.ru/p/c6291c62)

{{< button relref="/" >}}JS Core{{< /button >}}
{{< button relref="/" >}}Construct{{< /button >}}
{{< button relref="/" >}}Godot{{< /button >}}
{{< button relref="/" >}}Unity{{< /button >}}
{{< button relref="/" >}}Defold{{< /button >}}
