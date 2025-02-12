# test_task
Тестовое задание для PHP-программиста (Битрикс)

## Установка

Разархивируйте и скопируйте папку `local` в папку вашего проекта Битрикс.

## Компоненты

Описание компонентов, которые находятся в папке `local/components/`:

### [user.groups]

Компонент который выводит список групп пользователей и их описание

#### Вставка компонента:

```php
<?php
$APPLICATION->IncludeComponent(
    "custom:user.groups",
    "",
    [
        "CACHE_TIME" => 3600,    // Время кеширования
        "PAGE_TITLE" => "Список групп", // Заголовок страницы
    ]
);
?>
```

## Модули

Описание модулей, которые находятся в папке `local/modules/`:

### [usergroups]

Модуль, позволяющий вставить компонент user.groups в указанные страницы

#### Установка:

1. В панели управления Битрикс зайдите в "Настройки" → "Модули".
2. Установите модуль, выбрав его из списка доступных.
3. Настройте модуль в настройках модуля.
4. Вставьте следующий код в шаблон футера сайта:

```php
<?php
$isModuleEnabled = Bitrix\Main\Config\Option::get("usergroups", "module_enabled", "Y") === "Y";

$pagesList = Bitrix\Main\Config\Option::get("usergroups", "pages_list", "");

// Разбиваем строки на массив
if (!empty($pagesList)) {
    $pages = explode("\n", $pagesList);
    $pages = array_map('trim', $pages);
    $currentPage = $APPLICATION->GetCurPage();
}

if (!empty($pages) && in_array($currentPage, $pages) && $isModuleEnabled) {
    ?>
    <script type="text/javascript">
        document.addEventListener("DOMContentLoaded", function () {
            
            var workarea = document.getElementById("workarea");

            if (workarea) {
                var userGroupsComponent = document.createElement('div');
                userGroupsComponent.id = 'user-groups-component';

                workarea.appendChild(userGroupsComponent);

                fetch('/local/ajax/load_user_groups.php') 
                    .then(response => response.text())
                    .then(data => {

                        userGroupsComponent.innerHTML = data;
                    })
                    .catch(error => {
                        console.error('Ошибка при загрузке данных:', error);
                    });
            }
        });
    </script>
    <?php
}
?>
```
### [Название модуля 2]

Краткое описание модуля 2.

#### Установка:

1. Аналогично модулю 1.

## Использование

https://skr.sh/sU9PQPiPpo9

