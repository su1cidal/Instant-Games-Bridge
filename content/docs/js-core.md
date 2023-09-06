---
title: JS Core
type: docs
---

# **JS Core**

JS Core — это сердце SDK, содержит всю основную логику. Плагины под игровые движки (Unity, Godot, Construct) являются лишь надстройкой над JS Core.
JS Core можно использовать напрямую без каких-либо плагинов в веб-движках (PlayCanvas, PixiJS, Phaser и т.д.).

## **Подключение**

Скачайте файл `instant-games-bridge.js` со [страницы релизов на гитхаб](https://github.com/instant-games-bridge/instant-games-bridge/releases), добавьте в свой проект и подключите в `index.html`:

```
index.html
<html>
    <head>
        <script src="./instant-games-bridge.js"></script>
    </head>
    <body>...</body>
</html>
```

При запуске игры на поддерживаемых платформах SDK автоматически подключит нужные скрипты платформ. На неподдерживаемой платформе не будет каких-либо ошибок, подставится платформа-заглушка Mock и при вызове какого-либо запроса будет возвращаться `false`, `reject` и т.д.

## **Инициализация**

Перед использованием SDK нужно вызвать метод инициализации и дождаться его завершения.

```
index.html
<html>
    <hrad>...</head>
    <body>...</body>
    <script>
        bridge.initialize()
            .then(() => {
                // Инициализация прошла успешно, можно использовать SDK
            })
            .catch(error => {
                // Ошибка, что-то пошло не так
            })
        
        // Вы можете явно передать айди нужной платформы 
        // и тогда внутреннее автоопределение платформы будет пропущено
        bridge.initialize({ forciblySetPlatformId: bridge.PLATFORM_ID.YANDEX })
            .then(() => {
                // Инициализация прошла успешно, можно использовать SDK
            })
            .catch(error => {
                // Ошибка, что-то пошло не так
            })
        
        // Если вы выпускаете игру на Game Distribution
        // то необходимо указать её айди
        let bridgeOptions = { 
            platforms: { 
                'game_distribution': { 
                    gameId: '74473b0d8a42415db75fef0a005b9654' 
                } 
            } 
        }
        bridge.initialize(bridgeOptions)
            .then(() => {
                // Инициализация прошла успешно, можно использовать SDK
            })
            .catch(error => {
                // Ошибка, что-то пошло не так
            })
    </script>
</html>
```

## **Информация о платформе**

### **ID платформы**

~~~
bridge.platform.id
~~~

Возвращает ID платформы, на которой запущена игра в данный момент. Возможные значения: `vk`, `yandex`, `crazy_games`, `absolute_games`, `game_distribution`, `mock`.

### **Нативный SDK**

~~~
bridge.platform.sdk
~~~

Возвращает нативный SDK платформы.


|     Платформа     |       Содержание      |
|-------------------|-----------------------|
| VK                | [VK Bridge](https://dev.vk.com/bridge/overview)             |
| Yandex            | [Yandex Games SDK](https://yandex.ru/dev/games/doc/dg/sdk/sdk-about.html)      |
| Crazy Games       | [Crazy Games SDK](https://docs.crazygames.com/sdk/html5/)      |
| Absolute Games    | [Absolute Games SDK](https://silicon-wombat-25d.notion.site/Absolute-Games-SDK-45cb376372ae444599bb2f12d7618d02)    |
| Game Distribution | [Game Distribution SDK](https://gamedistribution.com/sdk) |
| Mock              | `null`                |

### **Язык**

~~~
bridge.platform.language
~~~

Если платформа предоставляет данные об языке пользователя — то это будет язык, который установлен у пользователя на платформе. Если не предоставляет — это будет язык браузера пользователя.

Формат: [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes). Пример: `ru`, `en`.

### **Параметр из адресной строки**

~~~
bridge.platform.payload
~~~

С помощью данного параметра можно в ссылку на игру встраивать какую-либо вспомогательную информацию.

|     Платформа     |       Формат ссылки   |
|-------------------|-----------------------|
| VK                | vk.com/app8056947#**your-info**|
| Yandex            | yandex.com/games/play/183100?**payload=your-info** |
| Crazy Games       | crazygames.com/game/example?**payload=your-info** |
| Absolute Games    | — |
| Game Distribution | — |
| Mock              | site.com/game?**payload=your-info** |


### **Информация о домене**

~~~
bridge.platform.tld
~~~

|     Платформа     |       Формат ссылки   |
|-------------------|-----------------------|
| VK                | `null` |
| Yandex            | `com`,`ru`, etc. |
| Crazy Games       | `null` |
| Absolute Games    | `null` |
| Game Distribution | `null` |
| Mock              | `null` |


### **Отправка сообщения платформе**

~~~
bridge.platform.sendMessage(message)
~~~

|        Сообщение        |                                                   Описание                                                   | Где поддерживается |
|-------------------------|--------------------------------------------------------------------------------------------------------------|--------------------|
| `game_ready`              | Игра загрузилась, все загрузочные экраны прошли, игрок может взаимодействовать с игрой.                      | `yandex`             |
| `in_game_loading_started` | Началась какая-либо загрузка внутри игры. Например, когда идёт загрузка уровня.                              | `crazy_games`        |
| `in_game_loading_stopped` | Загрузка внутри игры окончена.                                                                               | `crazy_games`        |
| `gameplay_started`        | Начался геймплей. Например, игрок зашёл в уровень с главного меню.                                           | `crazy_games`        |
| `gameplay_stopped`        | Геймплей закончился/приостановился. Например, при выходе с уровня в главное меню, открытии меню паузы и т.д. | `crazy_games`        |
| `player_got_achievement`  | Игрок достиг особенного момента. Например, победил босса, установил рекорд и т.д.                            | `crazy_games`        |

## **Информация о девайсе**

### **Тип девайса**

~~~
bridge.device.type
~~~

Возвращает тип девайса, с которого пользователь запустил игру. Возможные значения: `mobile`, `tablet`, `desktop`, `tv`.

## **Информация об игроке**

### **Поддержка авторизации**

~~~
bridge.player.isAuthorizationSupported
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true`               |
| Yandex            | `true`               |
| Crazy Games       | `false`             |
| Absolute Games    | `true`               |
| Game Distribution | `false`              |
| Mock              | `false`              |

### **Авторизован ли игрок в данный момент**

~~~
bridge.player.isAuthorized
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true`               |
| Yandex            | `true`/`false`      |
| Crazy Games       | `false`              |
| Absolute Games    | `true`/`false`      |
| Game Distribution | `false`              |
| Mock              | `false`              |

### **ID игрока**

~~~
bridge.player.id
~~~

Если авторизация поддерживается на платформе и игрок авторизован в данный момент – возвращает его ID на платформе, иначе — `null`.

### **Имя игрока**

~~~
bridge.player.name
~~~

| Платформа         | Возможные значения                                                                      |
|-------------------|-----------------------------------------------------------------------------------------|
| VK                | Имя игрока                                                                              |
| Yandex            | Если игрок авторизован и дал игре доступ к личной информации — имя игрока, иначе — `null` |
| Crazy Games       | `null`                                                                                    |
| Absolute Games    | Если игрок авторизован — имя игрока, иначе — `null`                                       |
| Game Distribution | `null`                                                                                    |
| Mock              | `null`                                                                                    |

### **Аватар игрока**

~~~
bridge.player.photos
~~~

Массив аватаров игрока, упорядоченный по возрастанию разрешения.

| Платформа         | Возможные значения                                                                                                                 |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| VK                | Массив ссылок на изображения                                                                                                       |
| Yandex            | Если игрок авторизован в данный момент и дал игре доступ к личной информации — массив ссылок на изображения, иначе — пустой массив |
| Crazy Games       | Пустой массив                                                                                                                      |
| Absolute Games    | Если игрок авторизован в данный момент — массив ссылок на изображения, иначе — пустой массив                                       |
| Game Distribution | Пустой массив                                                                                                                      |
| Mock              | Пустой массив                                                                                                                      |

### **Авторизация игрока**

```
// Необязательный параметр
let authorizationOptions = {
    'yandex': {
        scopes: true // Запросить у игрока доступ к имени и фото
    }
}

bridge.player.authorize(authorizationOptions)
    .then(() => {
        // Игрок успешно авторизован
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
```

| Платформа         | Возможные значения                                                                                                                                                           |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VK                | `resolved`                                                                                                                                                                   |
| Yandex            | Если игрок уже авторизован — `resolved`. Если не авторизован — показывается диалоговое окно авторизации. Далее `resolved` / `rejected` зависит от того авторизуется игрок или нет. |
| Crazy Games       | `rejected`                                                                                                                                                                     |
| Absolute Games    | Если игрок уже авторизован или открылось окно авторизации — `resolved`. После авторизации происходит релоад страницы.  `rejected` в случае ошибки.                               |
| Game Distribution | `rejected`                                                                                                                                                                     |
| Mock              | `rejected`                                                                                                                                                                     |

## **Информация об игре**

### **Текущее состояние видимости**

~~~
bridge.game.visibilityState

// Отслеживать изменение состояния можно подписавшись на событие
bridge.game.on(bridge.EVENT_NAME.VISIBILITY_STATE_CHANGED, state => { console.log('Visibility state:', state) })
~~~

Возвращает текущее состояние видимости игры (вкладки с игрой). Возможные значения: `visible`, `hidden`.

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния видимости. Например, выключать звук в игре при `hidden` и включать при `visible`.
{{< /hint >}}

## **Пользовательские данные**

Есть два типа хранилища: локальное `local_storage` и внутреннее `platform_internal`. При записи в локальное — данные сохраняются у игрока на конкретном девайсе, при записи во внутреннее — данные сохраняются на серверах платформы.

### **Текущий тип хранилища**

~~~
bridge.storage.defaultType
~~~

| Платформа         | Возможные значения                                                   |
|-------------------|----------------------------------------------------------------------|
| VK                | `platform_internal`                                                    |
| Yandex            | Если игрок авторизован — `platform_internal`, если нет — `local_storage` |
| Crazy Games       | `local_storage`                                                        |
| Absolute Games    | Если игрок авторизован — `platform_internal`, если нет — `local_storage` |
| Game Distribution | `local_storage`                                                        |
| Mock              | `local_storage`                                                        |

### **Проверка на поддержку**

~~~
bridge.storage.isSupported(storageType)
~~~

| Платформа         | local_storage | platform_internal |
|-------------------|---------------|-------------------|
| VK                | `true`          | `true`              |
| Yandex            | `true`          | `true`              |
| Crazy Games       | `true`          | `false`             |
| Absolute Games    | `true`          | `true`              |
| Game Distribution | `true`          | `false`             |
| Mock              | `true`          | `false`             |

{{< hint danger >}}
**⚠️ Предупреждение**\
В VK максимальный срок хранения данных по ключу — 1 год.
{{< /hint >}}



### **Проверка на доступность**

~~~
bridge.storage.isAvailable(storageType)
~~~

| Платформа         | local_storage | platform_internal                               |
|-------------------|---------------|-------------------------------------------------|
| VK                | `true`        | `true`                                            |
| Yandex            | `true`        | Если игрок авторизован — `true`, если нет — `false` |
| Crazy Games       | `true`        | `false`                                           |
| Absolute Games    | `true`        | Если игрок авторизован — `true`, если нет — `false` |
| Game Distribution | `true`        | `false`                                           |
| Mock              | `true`        | `false`                                           |

### **Загрузка данных**

~~~
// Загрузить данные по ключу
bridge.storage.get('key')
    .then(data => {
        // Данные загружены и вы можете работать с ними
        // data = null если для такого ключа нет данных
        console.log('Data: ', data)
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })

// Загрузить данные по нескольким ключам
bridge.storage.get(['key_1', 'key2'])
    .then(data => {
        // Данные загружены и вы можете работать с ними
        // data[n] = null если для такого ключа нет данных
        console.log('Data: ', data)
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Сохранение данных**

~~~
// Сохранить данные по ключу
bridge.storage.set('key', 'value')
    .then(() => {
        // Данные успешно сохранены
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })

// Сохранить данные по нескольким ключам
bridge.storage.set(['key_1', 'key2'], ['value_1', 'value_2'])
    .then(() => {
        // Данные успешно сохранены
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Удаление данных**

~~~
// Удалить данные по ключу
bridge.storage.delete('key')
    .then(() => {
        // Данные успешно удалены
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
    
// Удалить данные по нескольким ключам
bridge.storage.delete(['key_1', 'key2'])
    .then(() => {
        // Данные успешно удалены
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

Все операции с данными взаимодействуют с текущим хранилища платформы. Вы можете передать вторым аргументом конкретный тип хранилища, который нужно использовать. Перед этим убедитесь что хранилище доступно.

~~~
let storageType = bridge.STORAGE_TYPE.LOCAL_STORAGE
bridge.storage.get('key', storageType)
bridge.storage.set('key', 'value', storageType)
bridge.storage.delete('key', storageType)

// Вы можете выбрать тип хранилища для каждой платформы отдельно
let options = {
    'vk': bridge.STORAGE_TYPE.PLATFORM_INTERNAL,
    'yandex': bridge.STORAGE_TYPE.LOCAL_STORAGE,
    'crazy_games': bridge.STORAGE_TYPE.LOCAL_STORAGE
}
bridge.storage.get('key', options)
bridge.storage.set('key', 'value', options)
bridge.storage.delete('key', options)
~~~
## **Реклама**

### **Banner**

#### Поддерживается ли баннер

~~~
bridge.advertisement.isBannerSupported
~~~

| Платформа         | Возможные значения                       | platform_internal                               |
|-------------------|------------------------------------------|-------------------------------------------------|
| VK                | На мобильных — `true`, на десктопе — false | `true`                                            |
| Yandex            | `true`                                     | Если игрок авторизован — `true`, если нет — `false` |
| Crazy Games       | `true`                                     | `false`                                           |
| Absolute Games    | `false`                                    | Если игрок авторизован — `true`, если нет — `false` |
| Game Distribution | `true`                                     | `false`                                           |
| Mock              | `false`                                    | `false`                                           |

{{< hint info >}}
**📝 Примечание**\
Чтобы баннеры работали в Yandex — необходимо их включить в настройках игры.
{{< /hint >}}

{{< hint info >}}
**📝 Примечание**\
Чтобы баннеры работали в Crazy Games — необходимо в `index.html` добавить контейнер `<div>` нужных размеров и передать его идентификатор в `showBanner()`. Подробнее про размеры: [https://docs.crazygames.com/sdk/html5/#responsive-banners](https://docs.crazygames.com/sdk/html5/#responsive-banners).
{{< /hint >}}

{{< hint info >}}
**📝 Примечание**\
Чтобы баннеры работали в Game Distribution — необходимо в `index.html` добавить контейнер `<div>` нужных размеров и передать его идентификатор в `showBanner()`. Подробнее про размеры: [https://github.com/GameDistribution/GD-HTML5/wiki/Display-Ads](https://github.com/GameDistribution/GD-HTML5/wiki/Display-Ads).
{{< /hint >}}

#### Показать баннер

~~~
let bannerOptions = {
    'vk': {
        position: 'top', // Необязательный параметр, по умолчанию = bottom
        layoutType: 'resize', // Необязательный параметр
        canClose: false // Необязательный параметр
    },
    'crazy_games': {
        containerId: 'div-container-id'
    },
    'game_distribution': {
        containerId: 'div-container-id'
    }
}
bridge.advertisement.showBanner(bannerOptions)
~~~

#### Скрыть баннер

~~~
bridge.advertisement.hideBanner()
~~~

#### Состояние баннера

~~~
bridge.advertisement.bannerState

// Отслеживать изменение состояния можно подписавшись на событие
bridge.advertisement.on(bridge.EVENT_NAME.BANNER_STATE_CHANGED, state => console.log('Banner state: ', state))
~~~

Текущее состояние баннера. Возможные значения: `loading`, `shown`, `hidden`, `failed`.

### **Interstitial**

Межстраничная реклама. Обычно показывается в момент загрузки уровня/поражения и т.д.

#### Минимальный интервал между показами

~~~
// Значение по умолчанию = 60 секунд
bridge.advertisement.minimumDelayBetweenInterstitial
~~~

Между показами Interstitial-рекламы нужно соблюдать временные интервалы. Например, Yandex просто не покажет рекламу если вызывать слишком часто, а VK будет показывать так часто, как вызывается метод.

Для удобства, в данном SDK есть встроенный механизм таймера между показами. Вам нужно лишь указать нужный интервал и можно дёргать метод показа рекламы сколько угодно.

~~~
// Вы можете указать интервал для каждой платформы отдельно
let delayOptions = {
    'vk': 60,
    'yandex': 180,
    'crazy_games': 60,
    'absolute_games': 60,
    'game_distribution': 60
    'mock': 0
}
// Либо один для всех:
let delayOptions = 60

bridge.advertisement.setMinimumDelayBetweenInterstitial(delayOptions)
~~~

#### Состояние рекламы

~~~
bridge.advertisement.interstitialState

// Отслеживать изменение состояния можно подписавшись на событие
bridge.advertisement.on(bridge.EVENT_NAME.INTERSTITIAL_STATE_CHANGED, state => console.log('Interstitial state: ', state))
~~~

Текущее состояние рекламы. Возможные значения: `loading`, `opened`, `closed`, `failed`. 

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы. Например, выключать звук в игре при `opened` и включать при `closed` и `failed`.
{{< /hint >}}

#### Показать рекламу

~~~
// Необязательный параметр. Игнорировать минимальный интервал для всех платформ.
let interstitialOptions = {
    ignoreDelay: true // По умолчанию = false
}

// Можно указать для каждой платформы отдельно, например:
let interstitialOptions = {
    'vk': {
        ignoreDelay: true
    },
    'yandex': {
        ignoreDelay: false
    },
    'crazy_games': {
        ignoreDelay: false
    },
    'absolute_games': {
        ignoreDelay: false
    },
    'game_distribution': {
        ignoreDelay: false
    }
}

bridge.advertisement.showInterstitial(interstitialOptions)
~~~

### **Rewarded**

Реклама за вознаграждение.

#### Состояние рекламы

~~~
bridge.advertisement.rewardedState

// Отслеживать изменение состояния можно подписавшись на событие
bridge.advertisement.on(bridge.EVENT_NAME.REWARDED_STATE_CHANGED, state => console.log('Rewarded state: ', state))
~~~

Текущее состояние рекламы. Возможные значения: `loading`, `opened`, `closed`, `rewarded`, `failed`.


{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы. Например, выключать звук в игре при `opened` и включать при `closed` и `failed`.
{{< /hint >}}

{{< hint danger >}}
**⚠️ Предупреждение**\
Награду игроку нужно выдавать только при наступлении состояния rewarded.
{{< /hint >}}

#### Показать рекламу

~~~
bridge.advertisement.showRewarded()
~~~

## **Социальные взаимодействия**

### **Поделиться**

~~~
bridge.social.isShareSupported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true`             |
| Yandex            | `false`            |
| Crazy Games       | `false`            |
| Absolute Games    | `false`            |
| Game Distribution | `false`            |
| Mock              | `false`            |

~~~
let shareOptions = {
    'vk': {
        link: 'https://vk.com/wordle.game'
    }
}
bridge.social.share(shareOptions)
    .then(() => {
        // Игрок успешно поделился
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Вступить в сообщество**

~~~
bridge.social.isJoinCommunitySupported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true`             |
| Yandex            | `false`            |
| Crazy Games       | `false`            |
| Absolute Games    | `false`            |
| Game Distribution | `false`            |
| Mock              | `false`            |

~~~
let joinCommunityOptions = {
    'vk': {
        groupId: '199747461'
    }
}
bridge.social.joinCommunity(joinCommunityOptions)
    .then(() => {
        // Игрок успешно вступил
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Пригласить друзей**

~~~
bridge.social.isInviteFriendsSupported
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true`             |
| Yandex            | `false`            |
| Crazy Games       | `false`            |
| Absolute Games    | `false`            |
| Game Distribution | `false`            |
| Mock              | `false`            |

~~~
bridge.social.inviteFriends()
    .then(() => {
        // Игрок успешно пригласил друзей
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Опубликовать пост**

~~~
bridge.social.isCreatePostSupported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true`             |
| Yandex            | `false`            |
| Crazy Games       | `false`            |
| Absolute Games    | `false`            |
| Game Distribution | `false`            |
| Mock              | `false`            |

~~~
let createPostOptions = {
    'vk': {
        message: 'Hello world!',
        attachments: 'photo-199747461_457239629'
    }
}
bridge.social.createPost(createPostOptions)
    .then(() => {
        // Игрок успешно опубликовал пост
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Добавить в избранное**

~~~
bridge.social.isAddToFavoritesSupported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true`             |
| Yandex            | `false`            |
| Crazy Games       | `false`            |
| Absolute Games    | `false`            |
| Game Distribution | `false`            |
| Mock              | `false`            |

~~~
bridge.social.addToFavorites()
    .then(() => {
        // Игрок успешно добавил игру в избранное
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Добавить на рабочий стол**

~~~
bridge.social.isAddToHomeScreenSupported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения                                                                                            |
|-------------------|---------------------------------------------------------------------------------------------------------------|
| VK                | Android: `true`.  Desktop, iOS: `false`.                                                                          |
| Yandex            | `true` / `false` Доступность опции зависит от девайса игрока, внутренних правил браузера и ограничений платформы. |
| Crazy Games       | `false`                                                                                                         |
| Absolute Games    | `false`                                                                                                         |
| Game Distribution | `false`                                                                                                         |
| Mock              | `false`                                                                                                         |

~~~
bridge.social.addToHomeScreen()
    .then(() => {
        // Игрок успешно добавил игру на рабочий стол
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Оценить игру**

~~~
bridge.social.isRateSupported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `false`             |
| Yandex            | `true`            |
| Crazy Games       | `false`            |
| Absolute Games    | `false`            |
| Game Distribution | `false`            |
| Mock              | `false`            |

~~~
bridge.social.rate()
    .then(() => {
        // Игрок успешно оценил игру
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Внешние ссылки**

~~~
bridge.social.isExternalLinksAllowed
~~~

Разрешены ли внешние ссылки на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true`             |
| Yandex            | `false`            |
| Crazy Games       | `true`            |
| Absolute Games    | `false`            |
| Game Distribution | `false`            |
| Mock              | `false`            |

## **Лидерборды**

### **Поддержка**

~~~
bridge.leaderboard.isSupported
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | `true*`              |
| Yandex            | `true`              |
| Crazy Games       | `false`              |
| Absolute Games    | `false`              |
| Game Distribution | `false`              |
| Mock              | `false`              |

{{< hint danger >}}
**⚠️ Предупреждение**\
\* В VK с клиента можно только показать нативный popup, для всего остального потребуется свой сервер.
{{< /hint >}}

### **Нативный popup**

~~~
bridge.leaderboard.isNativePopupSupported
~~~

Поддерживается ли нативный popup.

| Платформа         | Возможные значения                            |
|-------------------|-----------------------------------------------|
| VK                | Android, iOS, Mobile Web: `true` Desktop: `false` |
| Yandex            | `false`                                         |
| Crazy Games       | `false`                                         |
| Absolute Games    | `false`                                         |
| Game Distribution | `false`                                         |
| Mock              | `false`                                         |

~~~
// Показать нативный popup
let showNativePopupOptions = {
    'vk': {
        userResult: 42,
        global: true // По умолчанию = false
    }
}
bridge.leaderboard.showNativePopup(showNativePopupOptions)
    .then(() => {
        // Popup успешно показан
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### **Очки игрока**

#### Запись

~~~
bridge.leaderboard.isSetScoreSupported
~~~

Поддерживается ли запись очков игрока.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | false              |
| Yandex            | true               |
| Crazy Games       | false              |
| Absolute Games    | false              |
| Game Distribution | false              |
| Mock              | false              |

~~~
let setScoreOptions = {
    'yandex': {
        leaderboardName: 'YOU_LEADERBOARD_NAME',
        score: 42
    }
}
bridge.leaderboard.setScore(setScoreOptions)
    .then(() => {
        // Очки успешно записаны
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

#### Чтение

~~~
bridge.leaderboard.isGetScoreSupported
~~~

Поддерживается ли чтение очков игрока.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | false              |
| Yandex            | true               |
| Crazy Games       | false              |
| Absolute Games    | false              |
| Game Distribution | false              |
| Mock              | false              |

~~~
let getScoreOptions = {
    'yandex': {
        leaderboardName: 'YOU_LEADERBOARD_NAME',
    }
}
bridge.leaderboard.getScore(getScoreOptions)
    .then(score => {
           console.log('Score: ' + score)
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

### Записи таблицы

~~~
bridge.leaderboard.isGetEntriesSupported
~~~

Поддерживается ли чтение полной таблицы.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | false              |
| Yandex            | true               |
| Crazy Games       | false              |
| Absolute Games    | false              |
| Game Distribution | false              |
| Mock              | false              |

~~~
let getEntriesOptions = {
    'yandex': {
        leaderboardName: 'YOU_LEADERBOARD_NAME',
        includeUser: true, // Default = false
        quantityAround: 10, // Default = 5
        quantityTop: 10 // Default = 5
    }
}
bridge.leaderboard.getEntries(getEntriesOptions)
    .then(entries => {
        // Данные успешно получены
        entries.forEach(e => {
            console.log('ID: ' + e.id + ', name: ' + e.name + ', score: ' + e.score + ', rank: ' + e.rank + ', small photo: ' + e.photos[0])
        })
    })
    .catch(error => {
        // Ошибка, что-то пошло не так
    })
~~~

{{< button relref="/" >}}Введение{{< /button >}}
{{< button relref="/docs/construct" >}}Construct{{< /button >}}