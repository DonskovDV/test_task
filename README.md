# test_task
Тестовое задание для PHP-программиста (Битрикс) https://docs.google.com/document/d/1mNw8GnqV9Q7dqukekhRJJj0GwPDwPRkMlBmkUNeuxcs/edit?tab=t.0

## Установка

Разархивируйте и скопируйте папку `local` в папку вашего проекта Битрикс. Также для работы компонента из 2-го задания скопируйте папку groups в папку вашего проекта Битрикс.

## Компоненты

Описание компонентов, которые находятся в папке `local/components/`:

### [user.groups]

Компонент который выводит группу пользователей в виде таблицы с колонками: ID, Название группы, Описание Группы

#### Вставка компонента на страницу:

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

### [group.list]

Компонент который выводит группу пользователей в виде таблицы с колонками: ID, Название группы, Описание Группы, Ссылка на детальную страницу группы

#### Вставка компонента на страницу:

```php
<?php
$APPLICATION->IncludeComponent(
    "custom:groups.list",
    "",
    [
        "CACHE_TIME" => 3600,
        "PAGE_TITLE" => "Список групп",
    ]
);
?>
```

#### Дополнительное пояснение по компоненту:

Для того, чтобы работали ссылки на детальные страницы, необходимо в файле urlrewrite.php 
добавить обработку ЧПУ:
```
3 =>
    array (
        'CONDITION' => '#^/groups/([0-9]+)/#',
        'RULE' => 'GROUP_ID=$1',
        'ID' => '',
        'PATH' => '/groups/detail.php',
        'SORT' => 90,
    ),
    // Список групп (ставим позже)
    4 =>
    array (
        'CONDITION' => '#^/groups/#',
        'RULE' => '',
        'ID' => '',
        'PATH' => '/groups/index.php',
        'SORT' => 100,
    ),
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


## Использование

https://skr.sh/sU9PQPiPpo9

