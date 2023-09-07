---
title: Unity
type: docs
---

# **Unity**

## **Подключение**

Скачайте актуальный `.unitypackage` cо страницы релизов на [github.com](https://github.com/instant-games-bridge/instant-games-bridge-unity/releases) и импортируйте в проект.
Для корректной работы необходимо импортироавать все файлы, кроме папки `Examples`.
Ее можно импортировать по желанию.
В ней есть сцена с примерами использования.

## **Инициализация**

Когда игра загружена — плагин уже проинициализирован. Ничего дополнительно делать не нужно.

## **Сборка**

Для корректной работы плагина необходимо выбрать соответствующий `WebGL Template`.

<div style="text-align: center;">
	<img width="750" src="/images/unity/1.png">
</div>

Здесь есть ряд настроек внешнего вида:<br/>
`Overlay Background Color` — цвет фона.<br/>
`Progress Bar Background Color` — цвет фона загрузочной полосы.<br/>
`Progress Bar Fill Color` — цвет самой полосы.<br/>
`Game Distribution Game ID` — ID игры, если публикуете на GameDistribution.

Цвет задаётся в формате HEX. Например, чёрный — #000000. Для значения по умолчанию нужно поставить знак -.

Чтобы заменить иконку — просто замените файл `WebGLTemplates/Bridge/icon.png`.

{{< hint danger >}}
**⚠️ Предупреждение**\
Важно! При загрузке файлов на **Crazy Games** в пункте **Game Type** нужно выбрать **HTML5**, а не Unity.
{{< /hint >}}

## **Информация о платформе**

### **ID платформы**

~~~
Bridge.platform.id
~~~

Возвращает ID платформы, на которой запущена игра в данный момент.
Возможные значения: `VK`, `Yandex`, `CrazyGames`, `AbsoluteGames`, `GameDistribution`, `Mock`.

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
Bridge.platform.SendMessage(PlatformMessage.GameReady);
~~~

| Сообщение               | Описание                                                                                                     | Где поддерживается |
|-------------------------|--------------------------------------------------------------------------------------------------------------|--------------------|
| GameReady              | Игра загрузилась, все загрузочные экраны прошли, игрок может взаимодействовать с игрой.                      | `Yandex`             |
| InGameLoadingStarted | Началась какая-либо загрузка внутри игры. Например, когда идёт загрузка уровня.                              | `CrazyGames`        |
| InGameLoadingStopped | Загрузка внутри игры окончена.                                                                               | `CrazyGames`        |
| GameplayStarted        | Начался геймплей. Например, игрок зашёл в уровень с главного меню.                                           | `CrazyGames`        |
| GameplayStopped        | Геймплей закончился/приостановился. Например, при выходе с уровня в главное меню, открытии меню паузы и т.д. | `CrazyGames`        |
| PlayerGotAchievement  | Игрок достиг особенного момента. Например, победил босса, установил рекорд и т.д.                            | `CrazyGames`        |

## **Информация о девайсе**

### **Тип девайса**

~~~
Bridge.device.type
~~~

Возвращает тип девайса, с которого пользователь запустил игру.
Возможные значения: `Mobile`, `Tablet`, `Desktop`, `TV`.

## **Информация об игроке**

### **Поддержка авторизации**

~~~
Bridge.player.isAuthorizationSupported
~~~

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext green  >}}true{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext green  >}}true{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|


### **Авторизация игрока**

~~~
private void Start() {
    // Вариант №1
    Bridge.player.Authorize();
    
    // Вариант №2, с коллбеком о завершении
    Bridge.player.Authorize(OnAuthorizePlayerCompleted);

    // Вариант №3, с дополнительными параметрами
    var yandexScopes = true; // Запросить у игрока доступ к имени и фото
    var yandexOptions = new AuthorizeYandexOptions(yandexScopes);
    Bridge.player.Authorize(OnAuthorizePlayerCompleted, yandexOptions);
}

private void OnAuthorizePlayerCompleted(bool success) {
    if (success) {
         // Игрок успешно авторизован
    }
    else
    {
        // Ошибка, что-то пошло не так
    }
}
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
Bridge.game.visibilityState
~~~

Возвращает текущее состояние видимости игры (вкладки с игрой).
Возможные значения: `visible`, `hidden`.

~~~
// Изменение состояния видимости можно отслеживать, подписавшись на событие
private void Start()
{
    Bridge.game.visibilityStateChanged += OnGameVisibilityStateChanged;
}

private void OnGameVisibilityStateChanged(VisibilityState state)
{
    switch (state) {
        case VisibilityState.Visible:
            // Вкладка с игрой видима
            break;
        case VisibilityState.Hidden:
            // Вкладка с игрой скрыта
            break;
    }
}
~~~

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы.
Например, выключать звук в игре при `Hidden` и включать при `Visible`.
{{< /hint >}}

## **Пользовательские данные**

Есть два типа хранилища: локальное `LocalStorage ` и внутреннее `PlatformInternal`.
При записи в локальное — данные сохраняются у игрока на конкретном девайсе, при записи во внутреннее — данные сохраняются на серверах платформы.

### **Тип хранилища по умолчанию**

~~~
Bridge.storage.defaultType
~~~

Используется автоматически, если при работе с данными не указывать конкретный тип.

| Платформа         | Возможные значения                                                    |
|-------------------|-----------------------------------------------------------------------|
| VK                | `PlatformInternal`                                                     |
| Yandex            | Если игрок авторизован — `PlatformInternal`, если нет — `LocalStorage`  |
| Crazy Games       | `LocalStorage`                                                         |
| Absolute Games    | Если игрок авторизован — `PlatformInternal`, если нет — `LocalStorage`  |
| Game Distribution | `LocalStorage`                                                         |
| Mock              | `LocalStorage`                                                         |

### **Проверка на поддержку**

~~~
Bridge.storage.IsSupported(StorageType.LocalStorage)
Bridge.storage.IsSupported(StorageType.PlatformInternal)
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
Bridge.storage.IsAvailable(StorageType.LocalStorage)
Bridge.storage.IsAvailable(StorageType.PlatformInternal)
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
// Получить данные по ключу
private void Start()
{
    Bridge.storage.Get("level", OnStorageGetCompleted);
}

private void OnStorageGetCompleted(bool success, string data)
{
    // Загрузка произошла успешно
    if (success)
    {
        if (data != null) {
            Debug.Log(data);
        }
        else
        {
            // Данных по ключу level нет
        }
    }
    else
    {
        // Ошибка, что-то пошло не так
    }
}

// Получить данные по нескольким ключам
private void Start()
{
    Bridge.storage.Get(new List<string>() { "level", "coins" }, OnStorageGetCompleted);
}

private void OnStorageGetCompleted(bool success, List<string> data)
{
    // Загрузка произошла успешно
    if (success)
    {
        if (data[0] != null)
        {
            Debug.Log($"Level: {data[0]}");
        }
        else
        {
            // Данных по ключу level нет
        }
        
        if (data[1] != null)
        {
            Debug.Log($"Coins: {data[1]}");
        }
        else
        {
            // Данных по ключу coins нет
        }
    }
    else
    {
        // Ошибка, что-то пошло не так
    }
}

// Получить данные из конкретного хранилища
private void Start()
{
    Bridge.storage.Get("level", OnStorageGetCompleted, StorageType.LocalStorage);
}

private void OnStorageGetCompleted(bool success, string data)
{
    // Загрузка произошла успешно
    if (success)
    {
        if (data != null) {
            Debug.Log(data);
        }
        else
        {
            // Данных по ключу level нет
        }
    }
    else
    {
        // Ошибка, что-то пошло не так
    }
}
~~~

### **Сохранение  данных**

~~~
// Сохранить данные по ключу
private void Start()
{
    Bridge.storage.Set("level", "dungeon_123", OnStorageSetCompleted);
}

private void OnStorageSetCompleted(bool success)
{
    Debug.Log($"OnStorageSetCompleted, success: {success}");
}

// Сохранить данные по нескольким ключам
private void Start()
{
    var keys = new List<string>() { "level", "is_tutorial_completed", "coins" };
    var data = new List<object>() { "dungeon_123", true, 12 };
    Bridge.storage.Set(keys, data, OnStorageSetCompleted);
}

// Сохранить данные в конкретное хранилище
private void Start()
{
    Bridge.storage.Set("level", "dungeon_123", OnStorageSetCompleted, StorageType.LocalStorage);
}
~~~

### **Удаление данных**

~~~
// Удалить данные по ключу
private void Start()
{
    Bridge.storage.Delete("level", OnStorageDeleteCompleted);
}

private void OnStorageDeleteCompleted(bool success)
{
    Debug.Log($"OnStorageDeleteCompleted, success: {success}");
}

// Удалить данные по нескольким ключам
private void Start()
{
    var keys = new List<string>() { "level", "is_tutorial_completed", "coins" };
    Bridge.storage.Delete(keys, OnStorageDeleteCompleted);
}

// Удалить данные из конкретного хранилища
private void Start()
{
    Bridge.storage.Delete("level", OnStorageDeleteCompleted, StorageType.LocalStorage);
}
~~~

Если при работе с данными не передавать третьим аргументом тип хранилища, то используется тип хранилища по умолчанию `Bridge.storage.defaultType`.

## **Реклама**

### **Banner**

#### Поддерживается ли баннер

~~~
Bridge.advertisement.isBannerSupported
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
Bridge.advertisement.ShowBanner()
~~~

#### Скрыть  баннер

~~~
Bridge.advertisement.HideBanner()
~~~

#### Состояние баннера

~~~
Bridge.advertisement.bannerState
~~~

Текущее состояние баннера. Возможные значения: `Loading`, `Shown`, `Hidden`, `Failed`.

~~~
// Отслеживать изменение состояния можно подписавшись на событие
private void Start()
{
    Bridge.advertisement.bannerStateChanged += OnBannerStateChanged;
}

private void OnBannerStateChanged(BannerState state)
{
    Debug.Log(state);
}
~~~

### **Interstitial**

Межстраничная реклама. Обычно показывается в момент загрузки уровня/поражения и т.д.

#### Минимальный интервал между показами

~~~
// Значение по умолчанию = 60 секунд
Bridge.advertisement.minimumDelayBetweenInterstitial
~~~

Между показами Interstitial-рекламы нужно соблюдать временные интервалы.
Например, Yandex просто не покажет рекламу если вызывать слишком часто, а VK будет показывать так часто, как вызывается метод.

Для удобства, в данном SDK есть встроенный механизм таймера между показами.
Вам нужно лишь указать нужный интервал и можно дёргать метод показа рекламы сколько угодно.

~~~
private void Start() {
    // Установить минимальные интервал для всех платформ
    Bridge.advertisement.SetMinimumDelayBetweenInterstitial(30);
    
    // Установить минимальный интервал для конкретных платформ
    Bridge.advertisement.SetMinimumDelayBetweenInterstitial(
        new SetMinimumDelayBetweenInterstitialVkOptions(30),
        new SetMinimumDelayBetweenInterstitialCrazyGamesOptions(0));
}
~~~

#### Состояние рекламы

~~~
Bridge.advertisement.interstitialState
~~~

Текущее состояние рекламы. Возможные значения: `Loading`, `Opened`, `Closed`, `Failed`. 

~~~
// Отслеживать изменение состояния можно подписавшись на событие
private void Start()
{
    Bridge.advertisement.interstitialStateChanged += OnInterstitialStateChanged;
}

private void OnInterstitialStateChanged(InterstitialState state)
{
    Debug.Log(state);
}
~~~

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы.
Например, выключать звук в игре при `Opened ` и включать при `Closed ` и `Failed`.
{{< /hint >}}

#### Показать рекламу

~~~
private void Start() 
{
    // Необязательный параметр: 
    // игнорировать ли внутренний минимальный интервал между показами
    var ignoreDelay = true; // По умолчанию = false
    
    // Одинаково для всех платформ
    Bridge.advertisement.ShowInterstitial(ignoreDelay);
    
    // Под кажду платформу по-разному
    Bridge.advertisement.ShowInterstitial(
        new ShowInterstitialVkOptions(ignoreDelay),
        new ShowInterstitialYandexOptions(ignoreDelay),
        new ShowInterstitialCrazyGamesOptions(ignoreDelay));
}
~~~

### **Rewarded**

Реклама за вознаграждение.

#### Состояние рекламы

~~~
Bridge.advertisement.rewardedState
~~~

Текущее состояние рекламы. Возможные значения: `Loading`, `Opened`, `Closed`, `Rewarded`, `Failed`.

~~~
// Отслеживать изменение состояния можно подписавшись на событие
private void Start()
{
    Bridge.advertisement.rewardedStateChanged += OnRewardedStateChanged;
}

private void OnRewardedStateChanged(RewardedState state)
{
    Debug.Log(state);
}
~~~

{{< hint info >}}
**📝 Примечание**\
Нужно реагировать на изменение состояния рекламы.
Например, выключать звук в игре при `Opened ` и включать при `Closed ` и `Failed`.
{{< /hint >}}

{{< hint danger >}}
**⚠️ Предупреждение**\
Награду игроку нужно выдавать только при наступлении состояния `Rewarded`.
{{< /hint >}}

#### Показать рекламу

~~~
Bridge.advertisement.ShowRewarded()
~~~

## **Социальные взаимодействия**

### **Поделиться**

~~~
Bridge.social.isShareSupported
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
private void Start()
{
    var link = "https://vk.com/bouncy.motors";
    var options = new ShareVkOptions(link);
    
    // Вариант №1 - просто поделиться
    Bridge.social.Share(options);
    
    // Вариант №2 - с коллбеком о завершении
    Bridge.social.Share(OnShareCompleted, options);
}

private void OnShareCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

### **Вступить в сообщество**

~~~
Bridge.social.isJoinCommunitySupported
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
private void Start()
{
    var groupid = 199747461;
    var options = new JoinCommunityVkOptions(groupid);
    
    // Вариант №1 - просто вступить в сообщество
    Bridge.social.JoinCommunity(options);
    
    // Вариант №2 - с коллбеком о завершении
    Bridge.social.JoinCommunity(OnJoinCommunityCompleted, options);
}

private void OnJoinCommunityCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

### **Пригласить друзей**

~~~
Bridge.social.isInviteFriendsSupported
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
private void Start()
{
    // Вариант №1 - просто пригласить друзей
    Bridge.social.InviteFriends();
    
    // Вариант №2 - с коллбеком о завершении
    Bridge.social.InviteFriends(OnInviteFriendsCompleted);
}

private void OnInviteFriendsCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

### **Опубликовать пост**

~~~
Bridge.social.isCreatePostSupported
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
private void Start()
{
    var message = "Hello world!";
    var attachments = "photo - 199747461_457239629";
    CreatePostVkOptions options = new CreatePostVkOptions(message, attachments);
    
    // Вариант №1 - просто создать пост
    Bridge.social.CreatePost(options);
    
    // Вариант №2 - с коллбеком о завершении
    Bridge.social.CreatePost(OnCreatePostCompleted, options);
}

private void OnCreatePostCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

### **Добавить в избранное**

~~~
Bridge.social.isAddToFavoritesSupported
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
private void Start()
{
    // Вариант №1 - просто добавить в избранное
    Bridge.social.AddToFavorites();
    
    // Вариант №2 - с коллбеком о завершении
    Bridge.social.AddToFavorites(OnAddToFavoritesCompleted);
}

private void OnAddToFavoritesCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

### **Добавить на рабочий стол**

~~~
Bridge.social.isAddToHomeScreenSupported
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
private void Start()
{
    // Вариант №1 - просто добавить в избранное
    Bridge.social.AddToHomeScreen();
    
    // Вариант №2 - с коллбеком о завершении
    Bridge.social.AddToHomeScreen(OnAddToFavoritesCompleted);
}

private void OnAddToHomeScreenCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

### **Оценить игру**

~~~
Bridge.social.isRateSupported
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
private void Start()
{
    // Вариант №1 - просто оценить
    Bridge.social.Rate();
    
    // Вариант №2 - с коллбеком о завершении
    Bridge.social.Rate(OnRateCompleted);
}

private void OnRateCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

### **Внешние ссылки**

~~~
Bridge.social.isExternalLinksAllowed
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
Bridge.leaderboard.isSupported
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
Bridge.leaderboard.isNativePopupSupported
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
private void Start()
{
    var userResult = 42;
    var showGlobal = true;
    var options = new ShowNativePopupVkOptions(userResult, showGlobal);
    
    // Вариант №1 - просто показать нативный popup
    Bridge.leaderboard.ShowNativePopup(options);
        
    // Вариант №2 - с коллбеком о завершении
    Bridge.leaderboard.ShowNativePopup(OnShowNativePopupCompleted, options);
}

private void OnShowNativePopupCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

### **Очки игрока**

#### Запись

~~~
Bridge.leaderboard.isSetScoreSupported
~~~

Поддерживается ли запись очков игрока с клиента.

| Платформа         | Возможные значения |
|-------------------|--------------------|
| VK                | {{< colourtext red  >}}false{{< /colourtext >}}|
| Yandex            | {{< colourtext green  >}}true{{< /colourtext >}}|
| Crazy Games       | {{< colourtext red  >}}false{{< /colourtext >}}|
| Absolute Games    | {{< colourtext red  >}}false{{< /colourtext >}}|
| Game Distribution | {{< colourtext red  >}}false{{< /colourtext >}}|
| Mock              | {{< colourtext red  >}}false{{< /colourtext >}}|

~~~
private void Start()
{
    var score = 42;
    var leaderboardName = "leaderboardName";
    var options = new SetScoreYandexOptions(score, leaderboardName);
    
    // Вариант №1 - просто записать очки игрока
    Bridge.leaderboard.SetScore(options);
    
    // Вариант №2 - с коллбекос о завершении
    Bridge.leaderboard.SetScore(OnSetScoreCompleted, options);
}

private void OnSetScoreCompleted(bool success)
{
    if (success)
    {
        // Операция прошла успешно
    }
    else
    {
        // Произошла ошибка
    }
}
~~~

#### Чтение

~~~
Bridge.leaderboard.isGetScoreSupported
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
private void Start()
{
    var leaderboardName = "leaderboardName";
    var options = new GetScoreYandexOptions(leaderboardName);
    
    // Коллбек получения очков обязательный
    Bridge.leaderboard.GetScore(OnGetScoreCompleted, options);
}
private void OnGetScoreCompleted(bool success, int score)
{
    if (success)
    {
        // Данные успешно получены
        Debug.Log(score);
    }
    else
    {
        // Что-то пошло не так
    }
}
~~~

### **Записи таблицы**

~~~
Bridge.leaderboard.isGetEntriesSupported
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
private void Start()
{
    var leaderboardName = "yourLeaderboard";
    var includeUser = true;
    var quantityTop = 10;
    var quantityAround = 10;
    var options = new GetEntriesYandexOptions(
        leaderboardName, 
        includeUser, 
        quantityTop, 
        quantityAround);
        
    Bridge.leaderboard.GetEntries(OnGetEntriesComplete, options);
}

private void OnGetEntriesComplete(bool success, List<LeaderboardEntry> entries)
{
    if (success)
    {
        foreach (var entry in entries)
        {
            Debug.Log($"Entry: id = {entry.id}, name = {entry.name}, rank = {entry.rank}, score = {entry.score}");
            
            foreach (var photo in entry.photos)
            {
                Debug.Log($"Entry {entry.id} photo {photo}");
            }
        }
    }
}
~~~
<div align = center>
{{< button relref="/docs/godot" >}}Godot{{< /button >}}

{{< button relref="/docs/defold" >}}Defold{{< /button >}}
<div/>