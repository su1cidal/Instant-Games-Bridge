---
title: Construct
type: docs
---

# **Construct**

## **Установка**

Скачайте актуальную версию `instant-games-bridge.c3addon` со [страницы релизов на гитхаб](https://github.com/instant-games-bridge/instant-games-bridge-construct/releases) или на [construct.net](https://www.construct.net/en/make-games/addons/748/instant-games-bridge).

Перейдите в `Menu` → `View` → `Addon Manager` и нажмите `Install new addon...`, выберите скачанный файл и нажмите `Install` в появившемся окне.
 
Откройте проект, нажмите правой кнопкой мыши на папке `Object types` и выберите `Add new object type`.

<div style="text-align: center;">
	<img width="350" src="/images/construct/1.png">
</div>

Выберите `Instant Games Bridge` и нажмите `Insert`.

<div style="text-align: center;">
	<img width="600" src="/images/construct/2.png">
</div>

## **Настройка**

Установите значение `Use Worker` на `No` в настройках проекта.

<div style="text-align: center;">
	<img width="800" src="/images/construct/3.png">
</div>

Настройте `InstantGamesBridge` нужным образом.

<div style="text-align: center;">
	<img width="800" src="/images/construct/4.png">
</div>

{{< hint info >}}
**📝 Примечание**\
Для работы плагина требуется в проект добавить **`JS SDK`**
{{< /hint >}}

**Вариант №1 (рекомендуемый):**

Убрать галочку `Load SDK from CDN`.
Скачать файл `instant-games-bridge.js`  той же версии что и плагин со [страницы релизов на гитхаб](https://github.com/instant-games-bridge/instant-games-bridge/releases).
Импортировать в папку `Scripts`. Выбрать его и установить `Purpose` = `Main script`.

<div style="text-align: center;">
	<img width="800" src="/images/construct/5.png">
</div>

**Вариант №2:**

Поставить галочку `Load SDK from CDN`.
Тогда `JS SDK` будет грузиться автоматически с гитхаба.
В `Custom CDN URL` можно указать свой `URL` откуда грузить `SDK`.
Данный вариант не будет работать на Yandex.

## **Инициализация**

Перед использованием плагина — его необходимо инициализировать.

**Вариант №1 (рекомендуемый):**

Установить галочку `Initialize On Load` в настройках плагина.
Тогда иницализация будет происходить автоматически ещё до старта игры.

**Вариант №2:**
Добавить экшн `Initialize`.
Важно дождаться его завершения.
Это можно сделать двумя способами: с помощью экшена `Wait for previous actions to complete` либо триггера `On Initialization Completed`.

<div style="text-align: center;">
	<img width="800" src="/images/construct/6.png">
</div>

## **Информация о платформе**

### **ID платформы**

Получать информацию о платформе можно либо через условия, либо в выражениях.

#### Через условия:

<div style="text-align: center;">
	<img width="500" src="/images/construct/7.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/8.png">
</div>

#### Через выражение:

~~~
InstantGamesBridge.PlatformId
~~~

Возвращает ID платформы, на которой запущена игра в данный момент.
Возможные значения: `vk`, `yandex`, `crazy_games`, `absolute_games`, `game_distribution`, `mock`.

### **Язык**

~~~
InstantGamesBridge.PlatformLanguage
~~~

Если платформа предоставляет данные об языке пользователя — то это будет язык, который установлен у пользователя на платформе.
Если не предоставляет — это будет язык браузера пользователя.

Формат: [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes). Пример: `ru`, `en`.

### **Параметр из адресной строки**

~~~
InstantGamesBridge.PlatformPayload
~~~

С помощью данного параметра можно в ссылку на игру встраивать какую-либо вспомогательную информацию.

| Платформа         | Формат ссылки                                  |
|-------------------|------------------------------------------------|
| VK                | vk.com/app8056947#**your-info**                    |
| Yandex            | yandex.com/games/play/183100?**payload=your-info** |
| Crazy Games       | crazygames.com/game/example?**payload=your-info**  |
| Absolute Games    | —                                              |
| Game Distribution | —                                              |
| Mock              | site.com/game?**payload=your-info**                |

### **Информация о домене**

~~~
InstantGamesBridge.PlatformTld
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

<div style="text-align: center;">
	<img width="600" src="/images/construct/9.png">
</div>

| Сообщение               | Описание                                                                                                     | Где поддерживается |
|-------------------------|--------------------------------------------------------------------------------------------------------------|--------------------|
| `Game Ready`              | Игра загрузилась, все загрузочные экраны прошли, игрок может взаимодействовать с игрой.                      | `yandex`           |
| `In-Game Loading Started` | Началась какая-либо загрузка внутри игры. Например, когда идёт загрузка уровня.                              | `crazy_games`      |
| `In-Game Loading Stopped` | Загрузка внутри игры окончена.                                                                               | `crazy_games`      |
| `Gameplay Started`        | Начался геймплей. Например, игрок зашёл в уровень с главного меню.                                           | `crazy_games`      |
| `Gameplay Stopped`        | Геймплей закончился/приостановился. Например, при выходе с уровня в главное меню, открытии меню паузы и т.д. | `crazy_games`      |
| `Player Got Achievement`  | Игрок достиг особенного момента. Например, победил босса, установил рекорд и т.д.                            | `crazy_games`      |

## **Информация о девайсе**

### **Тип девайса**

#### Через условие:

<div style="text-align: center;">
	<img width="600" src="/images/construct/10.png">
</div>

#### Через выражение:

~~~
InstantGamesBridge.DeviceType
~~~

Возвращает тип девайса, с которого пользователь запустил игру.
Возможные значения: `mobile`, `tablet`, `desktop`, `tv`.

## **Информация об игроке**

### **Поддержка авторизации**

<div style="text-align: center;">
	<img width="400" src="/images/construct/11.png">
</div>

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext green  >}}true{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

### **Авторизован ли игрок в данный момент**

<div style="text-align: center;">
	<img width="400" src="/images/construct/12.png">
</div>

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
InstantGamesBridge.PlayerId
~~~

Если авторизация поддерживается на платформе и игрок авторизован в данный момент – возвращает его ID на платформе, иначе — {{< colourtext red  >}}null{{< /colourtext >}}.

### **Имя игрока**

~~~
InstantGamesBridge.PlayerName
~~~

| Платформа         | Возможные значения                                                                      |
|-------------------|-----------------------------------------------------------------------------------------|
| VK                | Имя игрока                                                                              |
| Yandex            | Если игрок авторизован и дал игре доступ к личной информации — имя игрока, иначе — {{< colourtext red  >}}null{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}null{{< /colourtext >}}|
| Absolute Games    | Если игрок авторизован — имя игрока, иначе — {{< colourtext red  >}}null{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}null{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}null{{< /colourtext >}}|

### **Аватар игрока**

#### Количество аватаров

~~~
InstantGamesBridge.PlayerPhotosCount
~~~

#### Получить аватар по индексу

~~~
InstantGamesBridge.PlayerPhoto(index)
~~~

### **Авторизация игрока**

<div style="text-align: center;">
	<img width="700" src="/images/construct/13.png">
</div>

<div style="text-align: center;">
	<img width="500" src="/images/construct/14.png">
</div>

| Платформа         | Возможные значения IsLastAuthorizePlayerAuthorizedSuccessfully                                                                                                   |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | Если игрок уже авторизован — {{< colourtext green  >}}true{{< /colourtext >}}. Если не авторизован — показывается диалоговое окно авторизации. Далее {{< colourtext green  >}}true{{< /colourtext >}}/{{< colourtext red  >}}false{{< /colourtext >}} зависит от того авторизуется игрок или нет. |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | Если игрок уже авторизован или открылось окно авторизации — {{< colourtext green  >}}true{{< /colourtext >}}. После авторизации происходит релоад страницы.<br/>{{< colourtext red  >}}false{{< /colourtext >}} в случае ошибки.|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

## **Информация об игре**

### **Текущее состояние видимости**

~~~
InstantGamesBridge.VisibilityState
~~~

Возвращает текущее состояние видимости игры (вкладки с игрой).
Возможные значения: `visible`, `hidden`.

<div style="text-align: center;">
	<img width="500" src="/images/construct/15.png">
</div>

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния видимости. Например, выключать звук в игре при `hidden` и включать при `visible`.
{{< /hint >}}

## **Пользовательские данные**

Есть два типа хранилища: локальное `local_storage` и внутреннее `platform_internal`.
При записи в локальное — данные сохраняются у игрока на конкретном девайсе, при записи во внутреннее — данные сохраняются на серверах платформы.

### **Тип хранилища по умолчанию**

~~~
InstantGamesBridge.DefaultStorageType
~~~

Используется автоматически, если при работе с данными не указывать конкретный тип.

| Платформа         | Возможные значения                                                    |
|-------------------|-----------------------------------------------------------------------|
| VK                | `platform_internal`                                                     |
| Yandex            | Если игрок авторизован — `platform_internal`, если нет — `local_storage`  |
| Crazy Games       | `local_storage`                                                         |
| Absolute Games    | Если игрок авторизован — platform_internal, если нет — `local_storage`  |
| Game Distribution | `local_storage`                                                         |
| Mock              | `local_storage`                                                         |

### **Проверка на поддержку**

<div style="text-align: center;">
	<img width="400" src="/images/construct/16.png">
</div>

| Платформа         | LocalStorage | PlatformInternal |
|-------------------|--------------|------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext green  >}}true{{< /colourtext >}}             |
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext green  >}}true{{< /colourtext >}}             |
| Crazy Games       | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext red  >}}false{{< /colourtext >}}            |
| Absolute Games    | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext green  >}}true{{< /colourtext >}}             |
| Game Distribution | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext red  >}}false{{< /colourtext >}}            |
| Mock              | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext red  >}}false{{< /colourtext >}}            |

{{< hint danger >}}
**⚠️ Предупреждение**\
В VK максимальный срок хранения данных по ключу — 1 год.
{{< /hint >}}

### **Проверка на доступность**

<div style="text-align: center;">
	<img width="500" src="/images/construct/17.png">
</div>

| Платформа         | LocalStorage | Platforminternal                                |
|-------------------|--------------|-------------------------------------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext green  >}}true{{< /colourtext >}}                                            |
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}         | Если игрок авторизован — {{< colourtext green  >}}true{{< /colourtext >}}, если нет — {{< colourtext red  >}}false{{< /colourtext >}} |
| Crazy Games       | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext red  >}}false{{< /colourtext >}}                                           |
| Absolute Games    | {{< colourtext green  >}}true{{< /colourtext >}}         | Если игрок авторизован — {{< colourtext green  >}}true{{< /colourtext >}}, если нет — {{< colourtext red  >}}false{{< /colourtext >}} |
| Game Distribution | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext red  >}}false{{< /colourtext >}}                                           |
| Mock              | {{< colourtext green  >}}true{{< /colourtext >}}         | {{< colourtext red  >}}false{{< /colourtext >}}                                           |

### **Загрузка данных**

<div style="text-align: center;">
	<img width="700" src="/images/construct/18.png">
</div>

Вместо `Default` можно выбирать, из какого конкретно хранилища берутся данные.

<div style="text-align: center;">
	<img width="500" src="/images/construct/19.png">
</div>

После успешного получения данных значение хранится в:

~~~
InstantGamesBridge.StorageData
~~~

### **Сохранение данных**

<div style="text-align: center;">
	<img width="700" src="/images/construct/20.png">
</div>

Вместо `Default` можно выбирать, из какого конкретно хранилища берутся данные.

<div style="text-align: center;">
	<img width="500" src="/images/construct/21.png">
</div>

### **Удаление данных**

<div style="text-align: center;">
	<img width="700" src="/images/construct/22.png">
</div>

Вместо `Default` можно выбирать, из какого конкретно хранилища берутся данные.

<div style="text-align: center;">
	<img width="500" src="/images/construct/23.png">
</div>

## **Реклама**

### **Banner**

#### Поддерживается ли баннер

<div style="text-align: center;">
	<img width="500" src="/images/construct/24.png">
</div>

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
Чтобы баннеры работали в Crazy Games — необходимо в `index.html` добавить контейнер `<div>` нужных размеров и передать его идентификатор в `ShowBanner`.
Подробнее про размеры: [https://docs.crazygames.com/sdk/html5/#responsive-banners](https://docs.crazygames.com/sdk/html5/#responsive-banners).
{{< /hint >}}

{{< hint info >}}
**📝 Примечание**\
Чтобы баннеры работали в Game Distribution — необходимо в `index.html` добавить контейнер `<div>` нужных размеров и передать его идентификатор в `ShowBanner`.
Подробнее про размеры: [https://github.com/GameDistribution/GD-HTML5/wiki/Display-Ads](https://github.com/GameDistribution/GD-HTML5/wiki/Display-Ads).
{{< /hint >}}

#### Показать баннер

<div style="text-align: center;">
	<img width="500" src="/images/construct/25.png">
</div>

<div style="text-align: center;">
	<img width="750" src="/images/construct/26.png">
</div>

VK: положение баннера. Возможные значения: `top`, `bottom`. По умолчанию: `bottom`.

Crazy Games:  `id` контейнера `<div>`.

GameDistribution: `id` контейнера `<div>`.

#### Скрыть баннер

<div style="text-align: center;">
	<img width="450" src="/images/construct/27.png">
</div>

#### Состояние баннера

~~~
InstantGamesBridge.BannerState
~~~

Текущее состояние баннера. Возможные значения: `loading`, `shown`, `hidden`, `failed`.

<div style="text-align: center;">
	<img width="500" src="/images/construct/28.png">
</div>

### **Interstitial**

Межстраничная реклама. Обычно показывается в момент загрузки уровня/поражения и т.д.

#### Минимальный интервал между показами

~~~
// Значение по умолчанию = 60 секунд
InstantGamesBridge.MinimumDelayBetweenInterstitial
~~~

Между показами Interstitial-рекламы нужно соблюдать временные интервалы.
Например, Yandex просто не покажет рекламу если вызывать слишком часто, а VK будет показывать так часто, как вызывается метод.

Для удобства, в данном SDK есть встроенный механизм таймера между показами.
Вам нужно лишь указать нужный интервал и можно дёргать метод показа рекламы сколько угодно.

<div style="text-align: center;">
	<img width="750" src="/images/construct/29.png">
</div>

#### Состояние рекламы

#### Через триггеры

<div style="text-align: center;">
	<img width="500" src="/images/construct/30.png">
</div>

#### Через выражение

~~~
InstantGamesBridge.InterstitialState
~~~

Текущее состояние рекламы. Возможные значения: `loading`, `opened`, `closed`, `failed`. 

<div style="text-align: center;">
	<img width="500" src="/images/construct/31.png">
</div>

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы.
Например, выключать звук в игре при opened и включать при `closed` и `failed`.
{{< /hint >}}

#### Показать рекламу

<div style="text-align: center;">
	<img width="750" src="/images/construct/32.png">
</div>

Можно игнорировать таймер между показами рекламы для конкретных платформ:

<div style="text-align: center;">
	<img width="500" src="/images/construct/33.png">
</div>

### **Rewarded**

Реклама за вознаграждение.

#### Состояние рекламы

#### Через триггеры

<div style="text-align: center;">
	<img width="400" src="/images/construct/34.png">
</div>

#### Через выражение

~~~
InstantGamesBridge.RewardedState
~~~
Текущее состояние рекламы. Возможные значения: `loading`, `opened`, `closed`, `rewarded`, `failed`.

<div style="text-align: center;">
	<img width="400" src="/images/construct/35.png">
</div>

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

<div style="text-align: center;">
	<img width="400" src="/images/construct/36.png">
</div>

## **Социальные взаимодействия**

### **Поделиться**

<div style="text-align: center;">
	<img width="400" src="/images/construct/37.png">
</div>

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="500" src="/images/construct/38.png">
</div>

<div style="text-align: center;">
	<img width="600" src="/images/construct/39.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/40.png">
</div>

### **Вступить в сообщество**

<div style="text-align: center;">
	<img width="350" src="/images/construct/41.png">
</div>

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="500" src="/images/construct/42.png">
</div>

<div style="text-align: center;">
	<img width="500" src="/images/construct/43.png">
</div>

<div style="text-align: center;">
	<img width="500" src="/images/construct/44.png">
</div>

### **Пригласить друзей**

<div style="text-align: center;">
	<img width="350" src="/images/construct/45.png">
</div>

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="500" src="/images/construct/46.png">
</div>

<div style="text-align: center;">
	<img width="500" src="/images/construct/47.png">
</div>

### **Опубликовать пост**

<div style="text-align: center;">
	<img width="350" src="/images/construct/48.png">
</div>

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="500" src="/images/construct/49.png">
</div>

<div style="text-align: center;">
	<img width="700" src="/images/construct/50.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/51.png">
</div>

### **Добавить в избранное**

<div style="text-align: center;">
	<img width="350" src="/images/construct/52.png">
</div>

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="730" src="/images/construct/53.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/54.png">
</div>

### **Добавить на рабочий стол**

<div style="text-align: center;">
	<img width="500" src="/images/construct/55.png">
</div>

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения                                                                                            |
|-------------------|---------------------------------------------------------------------------------------------------------------|
| VK                | Android: {{< colourtext green  >}}true{{< /colourtext >}}. <br/>Desktop, iOS: {{< colourtext red  >}}false{{< /colourtext >}}.|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}} / {{< colourtext red  >}}false{{< /colourtext >}}<br/> Доступность опции зависит от девайса игрока, внутренних правил браузера и ограничений платформы.|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="500" src="/images/construct/56.png">
</div>

### **Оценить игру**

<div style="text-align: center;">
	<img width="500" src="/images/construct/57.png">
</div>

Поддерживается ли функционал на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="400" src="/images/construct/58.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/59.png">
</div>

### **Внешние ссылки**

<div style="text-align: center;">
	<img width="500" src="/images/construct/60.png">
</div>

Разрешены ли внешние ссылки на платформе.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}|
| Crazy Games       | {{< colourtext green  >}}true{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

## **Лидерборды**

### **Поддержка**

<div style="text-align: center;">
	<img width="350" src="/images/construct/61.png">
</div>

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

<div style="text-align: center;">
	<img width="350" src="/images/construct/62.png">
</div>

Поддерживается ли нативный popup.

| Платформа         | Возможные значения                            |
|-------------------|-----------------------------------------------|
| VK                | Android, iOS, Mobile Web: {{< colourtext green  >}}true{{< /colourtext >}}<br/>Desktop: {{< colourtext red  >}}false{{< /colourtext >}} |
| Yandex            | {{< colourtext red  >}}false{{< /colourtext >}}                                         |
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}                                         |
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}                                         |
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}                                         |
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}                                         |

<div style="text-align: center;">
	<img width="500" src="/images/construct/63.png">
</div>

<div style="text-align: center;">
	<img width="700" src="/images/construct/64.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/65.png">
</div>

### **Очки игрока**

#### Запись

<div style="text-align: center;">
	<img width="500" src="/images/construct/66.png">
</div>

Поддерживается ли запись очков игрока.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="500" src="/images/construct/67.png">
</div>

<div style="text-align: center;">
	<img width="700" src="/images/construct/68.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/69.png">
</div>

#### Чтение

<div style="text-align: center;">
	<img width="400" src="/images/construct/70.png">
</div>

Поддерживается ли чтение очков игрока.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="500" src="/images/construct/71.png">
</div>

<div style="text-align: center;">
	<img width="700" src="/images/construct/72.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/73.png">
</div>

После получения очков игрока значение хранится в

~~~
InstantGamesBridge.LeaderboardPlayerScore
~~~

### **Записи таблицы**

<div style="text-align: center;">
	<img width="500" src="/images/construct/74.png">
</div>

Поддерживается ли чтение полной таблицы.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

<div style="text-align: center;">
	<img width="500" src="/images/construct/75.png">
</div>

<div style="text-align: center;">
	<img width="700" src="/images/construct/76.png">
</div>

<div style="text-align: center;">
	<img width="400" src="/images/construct/77.png">
</div>

После удачного получения записей лидерборда, их значения можно получить используя:

~~~
// Количество записей
InstantGamesBridge.LeaderboardEntriesCount

// Имя игрока в записи под номером index
InstantGamesBridge.LeaderboardEntryPlayerName(int index)

// Очки игрока в записи под номером index
InstantGamesBridge.LeaderboardEntryPlayerScore(int index)

// Ранг (место) игрока в записи под номером index
InstantGamesBridge.LeaderboardEntryPlayerRank(int index)

// Количество фотографий у игрока в записи под номером index
InstantGamesBridge.LeaderboardEntryPlayerPhotosCount(int index)

// Фотография игрока под номером photoIndex в записи под номером entryIndex
InstantGamesBridge.LeaderboardEntryPlayerPhoto(int entryIndex, int photoIndex)
~~~
<div align = center>
{{< button relref="/docs/js-core" >}}JS Core{{< /button >}}
{{< button relref="/docs/godot" >}}Godot{{< /button >}}
<div/>