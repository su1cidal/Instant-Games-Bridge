---
title: Godot
type: docs
---

# **Godot**

## **Подключение**

Скачайте актуальный архив `nstant_games_bridge.zip` со [страницы релизов на гитхаб](https://github.com/instant-games-bridge/instant-games-bridge-godot/releases), разархивируйте его и положите содержимое в `res://addons`:

<div style="text-align: center;">
	<img width="500" src="/images/godot/1.png">
</div>

Включите плагин в настройках проекта:

<div style="text-align: center;">
	<img width="700" src="/images/godot/2.png">
</div>

## **Инициализация**

Когда игра загружена — плагин уже проинициализирован. Ничего дополнительно делать не нужно.

## **Сборка**

Для корректной работы плагина необходимо выбрать соответствующий HTML-шаблон. Укажите в настройках экспорта в пункте Custom HTML Shell:

~~~
res://addons/instant_games_bridge/template/index.html
~~~

<div style="text-align: center;">
	<img width="700" src="/images/godot/3.png">
</div>

## **Информация о платформе**

### **ID платформы**

~~~
Bridge.platform.id
~~~

Возвращает ID платформы, на которой запущена игра в данный момент.
Возможные значения: `vk`, `yandex`, `crazy_games`, `absolute_games`, `game_distribution`, `mock`.

### **Язык**

~~~
Bridge.platform.language
~~~

Если платформа предоставляет данные об языке пользователя — то это будет язык, который установлен у пользователя на платформе.
Если не предоставляет — это будет язык браузера пользователя.
Формат: [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes). Пример: `ru`, `en`.

### **Параметр из адресной строки**

~~~
Bridge.platform.payload
~~~

С помощью данного параметра можно в ссылку на игру встраивать какую-либо вспомогательную информацию.

| Платформа         | Формат ссылки                                  |
|-------------------|------------------------------------------------|
| VK                | vk.com/app8056947#**your-info**                   |
| Yandex            | yandex.com/games/play/183100?**payload=your-info**|
| Crazy Games       | crazygames.com/game/example?**payload=your-info**|
| Absolute Games    | —                                              |
| Game Distribution | —                                              |
| Mock              | site.com/game?**payload=your-info**             |

### **Информация о домене**

~~~
Bridge.platform.tld
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}null{{< /colourtext >}}|
| Yandex            | `com`, `ru`, etc.      |
| Crazy Games       | {{< colourtext red  >}}null{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}null{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}null{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}null{{< /colourtext >}}|

### **Отправка сообщения платформе**

~~~
Bridge.platform.send_message("game_ready")

# Для удобства можно использовать константы
Bridge.platform.send_message(Bridge.PlatformMessage.GAME_READY)
~~~

| Сообщение               | Описание                                                                                                     | Где поддерживается |
|-------------------------|--------------------------------------------------------------------------------------------------------------|--------------------|
| game_ready              | Игра загрузилась, все загрузочные экраны прошли, игрок может взаимодействовать с игрой.                      | `yandex`             |
| in_game_loading_started | Началась какая-либо загрузка внутри игры. Например, когда идёт загрузка уровня.                              | `crazy_games`        |
| in_game_loading_stopped | Загрузка внутри игры окончена.                                                                               | `crazy_games`        |
| gameplay_started        | Начался геймплей. Например, игрок зашёл в уровень с главного меню.                                           | `crazy_games`        |
| gameplay_stopped        | Геймплей закончился/приостановился. Например, при выходе с уровня в главное меню, открытии меню паузы и т.д. | `crazy_games`        |
| player_got_achievement  | Игрок достиг особенного момента. Например, победил босса, установил рекорд и т.д.                            | `crazy_games`        |

## **Информация о девайсе**

### **Тип девайса**

~~~
Bridge.device.type
~~~

Возвращает тип девайса, с которого пользователь запустил игру.
Возможные значения: `mobile`, `tablet`, `desktop`, `tv`.

## **Информация об игроке**

### **Поддержка авторизации**

~~~
Bridge.player.is_authorization_supported
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext green  >}}true{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

### **Авторизован ли игрок в данный момент**

~~~
Bridge.player.is_authorized
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}/{{< colourtext red  >}}false{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext green  >}}true{{< /colourtext >}}/{{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|


### **ID игрока**

~~~
Bridge.player.id
~~~

Если авторизация поддерживается на платформе и игрок авторизован в данный момент – возвращает его ID на платформе, иначе — {{< colourtext red  >}}null{{< /colourtext >}}.

#### Имя игрока

~~~
Bridge.player.name
~~~

| Платформа         | Возможные значения                                                                      |
|-------------------|-----------------------------------------------------------------------------------------|
| VK                | `Имя игрока`                                                                              |
| Yandex            | Если игрок авторизован и дал игре доступ к личной информации — `имя игрока`, иначе — {{< colourtext red  >}}null{{< /colourtext >}} |
| Crazy Games       | {{< colourtext red  >}}null{{< /colourtext >}}                                                                                    |
| Absolute Games    | Если игрок авторизован — `имя игрока`, иначе — {{< colourtext red  >}}null{{< /colourtext >}}                                       |
| Game Distribution | {{< colourtext red  >}}null{{< /colourtext >}}                                                                                    |
| Mock              | {{< colourtext red  >}}null{{< /colourtext >}}                                                                                    |


### **Аватар игрока**

~~~
Bridge.player.photos
~~~

Массив аватаров игрока, упорядоченный по возрастанию разрешения.

| Платформа         | Возможные значения                                                                                                                 |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| VK                | `Массив ссылок на изображения`                                                                                                       |
| Yandex            | Если игрок авторизован в данный момент и дал игре доступ к личной информации — `массив ссылок на изображения`, иначе — `пустой массив` |
| Crazy Games       | `Пустой массив`                                                                                                                      |
| Absolute Games    | Если игрок авторизован в данный момент — `массив ссылок на изображения`, иначе — `пустой массив`                                       |
| Game Distribution | `Пустой массив`                                                                                                                      |
| Mock              | `Пустой массив`                                                                                                                      |

### **Авторизация игрока**

~~~
func _ready():
    Bridge.player.authorize(funcref(self, "_on_player_authorize_completed"))
	
func _on_player_authorize_completed(success):
    if success:
        print("Authorized")
    else:
        print("Authorization error")
~~~

Массив аватаров игрока, упорядоченный по возрастанию разрешения.

| Платформа         | Возможные значения success                                                                                                                                       |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}                                                                                                                                                             |
| Yandex            | Если игрок уже авторизован — {{< colourtext green  >}}true{{< /colourtext >}}. Если не авторизован — `показывается диалоговое окно авторизации`. Далее {{< colourtext green  >}}true{{< /colourtext >}}/{{< colourtext red  >}}false{{< /colourtext >}} зависит от того авторизуется игрок или нет. |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}                                                                                                                                                            |
| Absolute Games    | Если игрок уже авторизован или открылось окно авторизации — {{< colourtext green  >}}true{{< /colourtext >}}. После авторизации происходит релоад страницы. <br/>{{< colourtext red  >}}false{{< /colourtext >}} в случае ошибки.                          |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}                                                                                                                                                            |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}                                                                                                                                                            |

## **Информация об игре**

### **Текущее состояние видимости**

~~~
Bridge.game.visibility_state
~~~

Возвращает текущее состояние видимости игры (вкладки с игрой).
Возможные значения: `visible`, `hidden`.

~~~
# Отслеживать изменение состояния можно подключившись к сигналу
func _ready():
    Bridge.game.connect("visibility_state_changed", self, "_on_visibility_state_changed")

func _on_visibility_state_changed(state):
    print(state)
~~~

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы.
Например, выключать звук в игре при opened и включать при `closed` и `failed`.
{{< /hint >}}

## **Пользовательские данные**

Есть два типа хранилища: локальное `local_storage` и внутреннее `platform_internal`.
При записи в локальное — данные сохраняются у игрока на конкретном девайсе, при записи во внутреннее — данные сохраняются на серверах платформы.

### **Тип хранилища по умолчанию**

~~~
Bridge.storage.default_type
~~~

Используется автоматически, если при работе с данными не указывать конкретный тип.

| Платформа         | Возможные значения                                                    |
|-------------------|-----------------------------------------------------------------------|
| VK                | `platform_internal`                                                     |
| Yandex            | Если игрок авторизован — `platform_internal`, если нет — `local_storage`  |
| Crazy Games       | `local_storage`                                                         |
| Absolute Games    | Если игрок авторизован — `platform_internal`, если нет — `local_storage`  |
| Game Distribution | `local_storage`                                                         |
| Mock              | `local_storage`                                                         |

### **Проверка на поддержку**

~~~
Bridge.storage.is_supported("local_storage")
Bridge.storage.is_supported("platform_internal")

# Для удобства можно использовать константы
Bridge.storage.is_supported(Bridge.StorageType.LOCAL_STORAGE)
Bridge.storage.is_supported(Bridge.StorageType.PLATFORM_INTERNAL)
~~~

| Платформа        | local_storage  | platform_internal |
|------------------|----------------|-------------------|
| VK               | {{< colourtext green  >}}true{{< /colourtext >}}           | {{< colourtext green  >}}true{{< /colourtext >}}              |
| Yandex           | {{< colourtext green  >}}true{{< /colourtext >}}           | {{< colourtext green  >}}true{{< /colourtext >}}              |
| Crazy Games      | {{< colourtext green  >}}true{{< /colourtext >}}           | {{< colourtext red  >}}false{{< /colourtext >}}             |
| Absolute Games   | {{< colourtext green  >}}true{{< /colourtext >}}           | {{< colourtext green  >}}true{{< /colourtext >}}              |
| GameDistribution | {{< colourtext green  >}}true{{< /colourtext >}}           | {{< colourtext red  >}}false{{< /colourtext >}}             |
| Mock             | {{< colourtext green  >}}true{{< /colourtext >}}           | {{< colourtext red  >}}false{{< /colourtext >}}             |

{{< hint danger >}}
**⚠️ Предупреждение**\
В VK максимальный срок хранения данных по ключу — 1 год.
{{< /hint >}}

### **Проверка на доступность**

~~~
Bridge.storage.is_available("local_storage")
Bridge.storage.is_available("platform_internal")

# Для удобства можно использовать константы
Bridge.storage.is_available(Bridge.StorageType.LOCAL_STORAGE)
Bridge.storage.is_available(Bridge.StorageType.PLATFORM_INTERNAL)
~~~

| Платформа         | local_storage | platform_internal                               |
|-------------------|---------------|-------------------------------------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}          | {{< colourtext green  >}}true{{< /colourtext >}}                                            |
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}          | Если игрок авторизован — {{< colourtext green  >}}true{{< /colourtext >}}, если нет — {{< colourtext red  >}}false{{< /colourtext >}} |
| Crazy Games       | {{< colourtext green  >}}true{{< /colourtext >}}          | {{< colourtext red  >}}false{{< /colourtext >}}                                           |
| Absolute Games    | {{< colourtext green  >}}true{{< /colourtext >}}          | Если игрок авторизован — {{< colourtext green  >}}true{{< /colourtext >}}, если нет — {{< colourtext red  >}}false{{< /colourtext >}} |
| Game Distribution | {{< colourtext green  >}}true{{< /colourtext >}}          | {{< colourtext red  >}}false{{< /colourtext >}}                                           |
| Mock              | {{< colourtext green  >}}true{{< /colourtext >}}          | {{< colourtext red  >}}false{{< /colourtext >}}                                           |

### **Загрузка данных**

~~~
# Загрузить данные по ключу
func _ready():
    Bridge.storage.get("level", funcref(self, "_on_storage_get_completed"))

func _on_storage_get_completed(success, data):
    if success:
        if data != null:
            print(data)
        else:
            # Данных по ключу level нет
            print("Data is null")


# Загрузить данные сразу по нескольким ключам
func _ready():
    Bridge.storage.get(["level", "coins"], funcref(self, "_on_storage_get_completed"))

func _on_storage_get_completed(success, data):
    if success:
        if data[0] != null:
            print("Level: ", data[0])
        else:
            # Данных по ключу level нет
            print("Level is null")
		
        if data[1] != null:
            print("Coins: ", data[1])
        else:
            # Данных по ключу coins нет
            print("Coins is null")


# Загрузить данные из конкретного хранилища
Bridge.storage.get("level", funcref(self, "_on_storage_get_completed"), Bridge.StorageType.LOCAL_STORAGE)
~~~

### **Сохранение  данных**

~~~
# Сохранить данные по ключу
func _ready():
    Bridge.storage.set("level", "dungeon_123", funcref(self, "_on_storage_set_completed"))

func _on_storage_set_completed(success):
    print("On Storage Set Completed, success: ", success)


# Сохранить данные по нескольким ключам
Bridge.storage.set(["level", "is_tutorial_completed", "coins"], ["dungeon_123", true, 42], funcref(self, "_on_storage_set_completed"))


# Сохранить данные в конкретное хранилище
Bridge.storage.set("level", "dungeon_123", funcref(self, "_on_storage_set_completed"), Bridge.StorageType.LOCAL_STORAGE)
~~~

### **Удаление данных**

~~~
# Удалить данные по ключу
func _ready():
    Bridge.storage.delete("level", funcref(self, "_on_storage_delete_completed"))

func _on_storage_delete_completed(success):
    print("On Storage Delete Completed, success: ", success)


# Удалить данные по нескольким ключам
Bridge.storage.delete(["level", "is_tutorial_completed", "coins"], funcref(self, "_on_storage_delete_completed"))


# Удалить данные из конкретного хранилища
Bridge.storage.delete("level", funcref(self, "_on_storage_delete_completed"), Bridge.StorageType.LOCAL_STORAGE)
~~~

Если при работе с данными не передавать третьим аргументом тип хранилища, то используется тип хранилища по умолчанию (`Bridge.storage.default_type`).

## **Реклама**

### **Banner**

#### Поддерживается ли баннер

~~~
Bridge.advertisement.is_banner_supported
~~~

| Платформа         | Возможные значения                       |
|-------------------|------------------------------------------|
| VK                | На мобильных — {{< colourtext green  >}}true{{< /colourtext >}}, на десктопе — {{< colourtext red  >}}false{{< /colourtext >}} |
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}                                     |
| Crazy Games       | {{< colourtext green  >}}true{{< /colourtext >}}                                     |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}                                    |
| Game Distribution | {{< colourtext green  >}}true{{< /colourtext >}}                                     |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}                                    |

{{< hint info >}}
**📝 Примечание**\
Чтобы баннеры работали в Yandex — необходимо их включить в настройках игры.
{{< /hint >}}

{{< hint info >}}
**📝 Примечание**\
Чтобы баннеры работали в Crazy Games — необходимо в `index.html` добавить контейнер `<div>` нужных размеров.
Подробнее про размеры: [https://docs.crazygames.com/sdk/html5/#responsive-banners](https://docs.crazygames.com/sdk/html5/#responsive-banners).
{{< /hint >}}

{{< hint info >}}
**📝 Примечание**\
Чтобы баннеры работали в Game Distribution — необходимо в `index.html` добавить контейнер `<div>` нужных размеров.
Подробнее про размеры: [https://github.com/GameDistribution/GD-HTML5/wiki/Display-Ads](https://github.com/GameDistribution/GD-HTML5/wiki/Display-Ads).
{{< /hint >}}

#### Показать баннер

~~~
Bridge.advertisement.show_banner()
~~~

#### Скрыть  баннер

~~~
Bridge.advertisement.hide_banner()
~~~

#### Состояние баннера

~~~
Bridge.advertisement.banner_state
~~~

Текущее состояние баннера. Возможные значения: `loading`, `shown`, `hidden`, `failed`.

~~~
# Отслеживать изменение состояния можно подключившись к сигналу
func _ready():
    Bridge.advertisement.connect("banner_state_changed", self, "_on_banner_state_changed")

func _on_banner_state_changed(state):
    print(state)
~~~

### **Interstitial**

Межстраничная реклама. Обычно показывается в момент загрузки уровня/поражения и т.д.

#### Минимальный интервал между показами

~~~
# Значение по умолчанию = 60 секунд
Bridge.advertisement.minimum_delay_between_interstitial
~~~

Между показами Interstitial-рекламы нужно соблюдать временные интервалы.
Например, Yandex просто не покажет рекламу если вызывать слишком часто, а VK будет показывать так часто, как вызывается метод.

Для удобства, в данном SDK есть встроенный механизм таймера между показами.
Вам нужно лишь указать нужный интервал и можно дёргать метод показа рекламы сколько угодно.

~~~
Bridge.advertisement.set_minimum_delay_between_interstitial(30)
~~~

#### Состояние рекламы

~~~
Bridge.advertisement.interstitial_state
~~~

Текущее состояние рекламы. Возможные значения: `loading`, `opened`, `closed`, `failed`. 

~~~
# Отслеживать изменение состояния можно подключившись к сигналу
func _ready():
    Bridge.advertisement.connect("interstitial_state_changed", self, "_on_interstitial_state_changed")

func _on_interstitial_state_changed(state):
    print(state)
~~~

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы.
Например, выключать звук в игре при `opened` и включать при `closed` и `failed`.
{{< /hint >}}

#### Показать рекламу

~~~
# Необязательный параметр: игнорировать ли внутренний минимальный интервал между показами рекламы
var ignore_delay = true # По умолчанию = false
Bridge.advertisement.show_interstitial(ignore_delay) 
~~~

### **Rewarded**

Реклама за вознаграждение.

#### Состояние рекламы

~~~
Bridge.advertisement.rewarded_state
~~~

Текущее состояние рекламы. Возможные значения: `loading`, `opened`, `closed`, `rewarded`, `failed`.

~~~
# Отслеживать изменение состояния можно подключившись к сигналу
func _ready():
    Bridge.advertisement.connect("rewarded_state_changed", self, "_on_rewarded_state_changed")

func _on_rewarded_state_changed(state):
    print(state)
~~~

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы.
Например, выключать звук в игре при `opened` и включать при `closed` и `failed`.
{{< /hint >}}

{{< hint danger >}}
**⚠️ Предупреждение**\
Награду игроку нужно выдавать только при наступлении состояния `rewarded`.
{{< /hint >}}

#### Показать рекламу

~~~
Bridge.advertisement.show_rewarded()
~~~

## **Социальные взаимодействия**

### **Поделиться**

~~~
Bridge.social.is_share_supported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}               |
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}              |

~~~
func _ready():
    var link = "https://vk.com/bouncy.motors"
    var share_options = Bridge.ShareVkOptions.new(link)
    Bridge.social.share(share_options, funcref(self, "_on_share_completed"))

func _on_share_completed(success):
    print(success)
~~~

### **Вступить в сообщество**

~~~
Bridge.social.is_join_community_supported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}               |
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}              |

~~~
func _ready():
    var group_id = "199747461"
    var join_community_options = Bridge.JoinCommunityVkOptions.new(group_id)
    Bridge.social.join_community(join_community_options, funcref(self, "_on_join_community_completed"))
    
func _on_join_community_completed(success):
    print(success)
~~~

### **Пригласить друзей**

~~~
Bridge.social.is_invite_friends_supported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}               |
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}              |

~~~
func _ready():
    Bridge.social.invite_friends(funcref(self, "_on_invite_friends_completed"))
    
func _on_invite_friends_completed(success):
    print(success)
~~~

### **Опубликовать пост**

~~~
Bridge.social.is_create_post_supported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}               |
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}              |

~~~
func _ready():
    var message = "Hello world!"
    var attachments = "photo-199747461_457239629"
    var create_post_options = Bridge.CreatePostVkOptions.new(message, attachments)
    Bridge.social.create_post(create_post_options, funcref(self, "_on_create_post_completed"))
    
func _on_create_post_completed(success):
    print(success)
~~~

### **Добавить в избранное**

~~~
Bridge.social.is_add_to_favorites_supported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}               |
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}              |

~~~
func _ready():
    Bridge.social.add_to_favorites(funcref(self, "_on_add_to_favorites_completed"))
    
func _on_add_to_favorites_completed(success):
    print(success)
~~~

### **Добавить на рабочий стол**

~~~
Bridge.social.is_add_to_home_screen_supported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | Android: {{< colourtext green  >}}true{{< /colourtext >}}.<br/>Desktop, iOS: {{< colourtext red  >}}false{{< /colourtext >}}.              |
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}} / {{< colourtext red  >}}false{{< /colourtext >}}<br/>Доступность опции зависит от девайса игрока, внутренних правил браузера и ограничений платформы.             |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}              |

~~~
func _ready():
    Bridge.social.add_to_home_screen(funcref(self, "_on_add_to_home_screen_completed"))
    
func _on_add_to_home_screen_completed(success):
    print(success)
~~~

### **Оценить игру**

~~~
Bridge.social.is_rate_supported
~~~

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}               |
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}              |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}              |

~~~
func _ready():
    Bridge.social.rate(funcref(self, "_on_rate_completed"))
    
func _on_rate_completed(success):
    print(success)
~~~

### **Внешние ссылки**

~~~
Bridge.social.is_external_links_allowed
~~~

Разрешены ли внешние ссылки на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}               |
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Crazy Games       | {{< colourtext green  >}}true{{< /colourtext >}}              |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}              |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}              |

## **Лидерборды**

### **Внешние ссылки**

~~~
Bridge.leaderboard.is_supported
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext orange  >}}true*{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

{{< hint danger >}}
**⚠️ Предупреждение**\
\* В VK с клиента можно только показать нативный popup, для всего остального потребуется свой сервер.
{{< /hint >}}

### **Нативный popup**

~~~
Bridge.leaderboard.is_native_popup_supported
~~~

Поддерживается ли нативный popup.

| Платформа         | Возможные значения                            |
|-------------------|-----------------------------------------------|
| VK                | Android, iOS, Mobile Web: {{< colourtext green  >}}true{{< /colourtext >}}<br/>Desktop: {{< colourtext red  >}}false{{< /colourtext >}} |
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}                                         |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}                                         |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}                                         |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}                                         |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}                                         |

~~~
func _ready():
    var user_result = 42
    var global = true
    var show_native_popup_options = Bridge.ShowNativePopupVkOptions.new(user_result, global)
    Bridge.leaderboard.show_native_popup(create_post_options, funcref(self, "_on_show_native_popup_completed"))
    
func _on_show_native_popup_completed(success):
    print(success)
~~~

### **Очки игрока**

#### Запись

~~~
Bridge.leaderboard.is_set_score_supported
~~~

Поддерживается ли запись очков игрока.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

~~~
func _ready():
    var score = 42
    var leaderboard_name = "leaderboard_name"
    var set_score_options = Bridge.SetScoreYandexOptions.new(score, leaderboard_name)
    Bridge.leaderboard.set_score(set_score_options, funcref(self, "_on_set_score_completed"))
    
func _on_set_score_completed(success):
    print(success)
~~~

#### Чтение

~~~
Bridge.leaderboard.is_get_score_supported
~~~

Поддерживается ли чтение очков игрока.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

~~~
func _ready():
    var leaderboard_name = "leaderboard_name"
    var get_score_options = Bridge.GetScoreYandexOptions.new(leaderboard_name)
    Bridge.leaderboard.get_score(get_score_options, funcref(self, "_on_get_score_completed"))
    
func _on_get_score_completed(success, score):
    print(success)
    print(score)
~~~

### **Записи таблицы**

~~~
Bridge.leaderboard.is_get_entries_supported
~~~

Поддерживается ли чтение полной таблицы.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

~~~
func _ready():
    var leaderboard_name = "leaderboard_name"
    var include_user = true
    var quantity_around = 10
    var quantity_top = 10
    var get_entries_options = Bridge.GetEntriesYandexOptions.new(leaderboard_name, include_user, quantity_around, quantity_top)
    Bridge.leaderboard.get_entries(get_entries_options , funcref(self, "_on_get_entries_completed"))
    
func _on_get_entries_completed(success, entries):
    print(success)
    print(entries)
~~~
<div align = center>
{{< button relref="/docs/construct" >}}Construct{{< /button >}}

{{< button relref="/docs/unity" >}}Unity{{< /button >}}
<div/>