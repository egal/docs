# Загрузка файлов

[MiniIO](https://min.io/) – это популярное программное обеспечение с открытым исходным кодом 
для распределенного хранения объектов, совместимое с S3.

## Базовая загрузка файлов

1. В файле конфигурации файловой системы по адресу `config/filesystems.php` зарегистрируйтесь и настройте диск.
2. В модели с файлами нужно подключите трейт `Egal\ModelFileStoring\FileStoring`.

```php
   namespace App\Models;

   use Egal\Model\Model;
    use Egal\ModelFileStoring\FileStoring;

   /**
    * @property $id
    */
   class Document extends Model
   {

       use FileStoring;

   }
```

3. В модели укажите наименования полей с содержимым файла.

```php
   private array $contentNames = [
       'file'
   ];
```

4. Дополните модель полями на основании $contentNames.

```php
   /**
    * @property $id
    * @property string $file_path {@property-type field} {@validation-rules required|string}
    * @property string $file_url {@property-type fake-field}
    */
```

5. Выполните запрос на загрузку файла `Document/upload` с параметрами:

```json
   {
       "file_basename":  "example.txt",
       "contents": "Contents of file"
   }
```

В ответ должно прийти значение `path` для сохранения в объекте модели.

6. Сохранение объекта модели

Создайте или обновите модель, подставляя полученное значение `path` в поле пути до содержимого файла.

Например:

Выполните запрос `Document/create` с параметрами:

```json
    {
        "attributes": {
            "file_path": "path from upload result"
        }
    }
```

Для получения ссылки на файл выполните запрос на получение нужного объекта модели.
   
Например:

Выполните запрос `Document/getItem` с параметрами:

```json
    {
        "id": 1
    }
```

В ответ приходят данные:

```json
    {
        "id": 1,
        "file_url": "URL to your file",
        "updated_at": "2021-06-28T05:38:13.000000Z",
        "created_at": "2021-06-28T05:38:13.000000Z"
    }
```

## Файловые системы, совместимые с S3. Загрузка файлов 

1. Установите `league/flysystem-aws-s3-v3` версии `^1.0.0`.

```shell script
   composer require league/flysystem-aws-s3-v3:^1.0.0
```

2. В файле конфигурации файловой системы по адресу `config/filesystems.php` зарегистрируйтесь и настройте `s3` диск.

```php
   return [

       // ...

       'disks' => [

           // ...

           's3' => [
               'driver' => 's3',
               'key' => env('S3_ACCESS_KEY_ID'),
               'secret' => env('S3_SECRET_ACCESS_KEY'),
               'region' => env('S3_DEFAULT_REGION'),
               'bucket' => env('S3_BUCKET'),
               'url' => env('S3_URL'),
               'endpoint' => env('S3_ENDPOINT'),
               'visibility' => env('S3_VISIBILITY', 'private'),
           ],

           // ...

       ],

       // ...

   ];
```

3. Подключите трейт `Egal\ModelS3FileStoring\S3FileStoring` в вашу модель с файлами.

```php
   namespace App\Models;

   use Egal\Model\Model;
    use Egal\ModelS3FileStoring\S3FileStoring;

   /**
    * @property $id
    */
   class Document extends Model
   {

       use S3FileStoring;

   }
```

4. В модели укажите наименования полей с содержимым файла.

```php
   private array $contentNames = [
       'file'
   ];
```

5. Дополните модель полями на основании $contentNames.

```php
   /**
    * @property $id
    * @property string $file_path {@property-type field} {@validation-rules required|string}
    * @property string $file_url {@property-type fake-field}
    */
```

6. Выполните запрос для создания процесса загрузки `Document/createMultipartUpload` с параметрами:

```json
   {
       "file_basename":  "example.txt"
   }
```

В ответ приходят данные `path` and `upload_id`.

8. Разбейте файл на части. Каждая часть должна быть не меньше 5 мб, кроме последней.
9. Выполните запросы на загрузку каждой части файла  `Document/uploadPart` с параметрами:

```json
    {
        "upload_id": "upload_id from create multipart upload action result",
        "path": "path from create multipart upload action result",
        "part_number": "part number from 1 to 10000"
    }
```

10. Выполните запрос на завершение процесса загрузки файла на сервер `Document/completeMultipartUpload` с параметрами:

```json
     {
         "upload_id": "upload_id from create multipart upload action result",
         "path": "path from create multipart upload action result"
     }
```

В ответ должен прийти `path`.


10. Сохранение объекта модели

Создайте или обновите модель, подставляя полученное значение `path` в поле пути до содержимого файла.

Например:

Выполните запрос на создание `Document/create` с параметрами:

```json
    {
        "attributes": {
            "file_path": "path from upload result"
        }
    }
```

Для получения ссылки на файл выполните запрос на получение нужного объекта модели.

Например:

Выполните запрос на получение `Document/getItem` с параметрами:

```json
    {
        "id": 1
    }
```

В ответ приходят данные:

```json
    {
        "id": 1,
        "file_url": "URL to your file",
        "updated_at": "2021-06-28T05:38:13.000000Z",
        "created_at": "2021-06-28T05:38:13.000000Z"
    }
```

## Смена имени диска

По умолчанию модель использует диск, объявленный в конфигурации ключом `filesystem.default`.
Для смены диска объявите свойство модели:

```php
private bool $diskName = 'local';
```

## Отключение мутаторов полей url

> Допустим, вам нужно сохранить URL-адрес файла в базе данных, вы должны отключить мутаторы полей URL 
> и реализовать сохранение.

Для отключения мутаторов полей url объявите в модели свойство:

```php
private bool $needMutateUrlFields = false;
```

## Переопределение мутаторов полей url

Объявите метод в модели:

```php
protected function getContentUrl(string $contentName): string
{
    // TODO: Url fields mutator.
}
```

## Смена постфикса полей содержимого файла

Для смены постфикса пути файла объявите свойство в модели:

```php
private string $contentPathPropertyNamePostfix = '_path';
```

Для смены постфикса url файла объявите свойство в модели:

```php
private string $contentUrlPropertyNamePostfix = '_url';
```
