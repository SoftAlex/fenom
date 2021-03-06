Настройка
=========

## Configure

### Кеш шаблонов

```php
$fenom->setCompileDir($dir);
```

Задает имя каталога, в котором хранятся компилированные шаблоны. По умолчанию это `/tmp`. Каталог дожен быть доступен на запись.

### Параметры обработчика

Установка параметров через фабрику
```php
$fenom = Fenom::factory($tpl_dir, $compile_dir, $options);
```

Установка параметров через метод
```php
$fenom->setOptions($options);
```
В обоих случаях аргумет `$options` может быть массивом или битовой маской.
В массиве ключем должно быть название параметра, а значением — булевый флаг `true` (активировать) или `false` (деактивировать).
Битовая маска должна состоять из значений констант, согласно следующей таблице

| Название параметра     | Константа                 | Описание  | Эффект  |
| ---------------------- | ------------------------- | ------------ | ------- |
| *disable_methods*      | `Fenom::DENY_METHODS`     | отключает возможность вызова методов в шаблоне  | |
| *disable_native_funcs* | `Fenom::DENY_NATIVE_FUNCS`| отключает возможность использования функций PHP, за исключением разрешенных  | |
| *auto_reload*          | `Fenom::AUTO_RELOAD`      | автоматически пересобирать кеш шаблона если шаблон изменился | понижает производительность |
| *force_compile*        | `Fenom::FORCE_COMPILE`    | каждый раз пересобирать кеш шаблонов (рекомендуется только для отладки)| очень сильно понижает производительность |
| *disable_cache*        | `Fenom::DISABLE_CACHE`    | не кешировать компилированный шаблон | эпично понижает производительность |
| *force_include*        | `Fenom::FORCE_INCLUDE`    | стараться по возможности вставить код дочернего шаблона в родительский при подключении шаблона  | повышает производительность, увеличивает размер файлов в кеше, уменьшает количество файлов в кеше |
| *auto_escape*          | `Fenom::AUTO_ESCAPE`      | автоматически экранировать HTML сущности при выводе переменных в шаблон | понижает производительность |
| *force_verify*         | `Fenom::FORCE_VERIFY`     | автоматически проверять существование переменной перед использованием в шаблоне | понижает производительность |
| *disable_call*         | `Fenom::DENY_CALL`        | отключает возможность вызова статических методов и функций в шаблоне | |
| *disable_php_calls*    | `Fenom::DENY_PHP_CALLS`   | устаревшее название disable_call | |
| *disable_statics*      | `Fenom::DENY_STATICS`     | устаревшее название disable_call | |
| *strip*                | `Fenom::AUTO_STRIP`       | удаляет лишиние пробелы в шаблоне | уменьшает размер кеша |

```php
$fenom->setOptions(array(
    "compile_check" => true,
    "force_include" => true
));
// тоже самое что и
$fenom->setOptions(Fenom::AUTO_RELOAD | Fenom::FORCE_INCLUDE);
```

```php
$fenom->addCallFilter('View\Widget\*::get*')
```

**Замечание**
По умолчанию все параметры деактивированы.
