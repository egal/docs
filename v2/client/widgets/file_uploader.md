## File Uploader

Определяется тэгом:
```vue
<e-file-uploader></e-file-uploader>
```

### Параметры
File Uploader принимает 2 параметра в качестве пропа:
1. объект `data` который может содержать следующие параметры:
   если какой-то из параметров не передан, будет применено дефолтное значение.

| Параметр            |   Тип   |    По умолчанию     | Возможные значения                                              | Описание                                                                                             |
|---------------------|:-------:|:-------------------:|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| ``label``           | String  |       ``''``        | Любая строка                                                    | Label                                                                                                |
| ``helperText``      | String  |       ``''``        | Любая строка                                                    | Мелкий текст под счетчиком                                                                           |
| ``modelValue``      |  Array  |       ``[]``        | Массив файлов                                                   | Массив объектов с полями id, name, size, file_url  (можно использовать v-model)                      |
| ``validators``      |  Array  |       ``[]``        | Массив функций-валидаторов                                      | Проверка значения будет проходить через каждую функцию-валидатор                                     |
| ``maxFiles``        | Number  |        ``1``        | Любое число                                                     | Максимальное количество файлов                                                                       |
| ``maxSize``         | Number  |        ``0``        | Любое число                                                     | Максимальный размер файла в байтах (0 означает без ограничений)                                      |
| ``disabled``        | Boolean |      ``false``      | ``true``, ``false``                                             | Заблокировать инпут                                                                                  |
| ``deletable``       | Boolean |      ``true``       | ``true``, ``false``                                             | Возможность удалять файлы                                                                            |
| ``multiple``        | Boolean |      ``false``      | ``true``, ``false``                                             | Возможность загрузить несколько файлов                                                               |
| ``size``            | String  |       ``md``        | ``sm``, ``md``, ``lg``                                          | Размер счетчика                                                                                      |
| ``domain``          | String  |       ``''``        | Любая строка                                                    | Домейн для интеграции загрузки и удаления                                                            |
| ``microservice``    | String  |      ``core``       | Любая строка                                                    | Микросервис для интеграции загрузки и удаления                                                       |
| ``model``           | String  |    ``Document``     | Любая строка                                                    | Модель для интеграции загрузки и удаления                                                            |
| ``accept``          |  Array  |       ``[]``        | ``['image/png', 'image/gif', 'image/jpeg', 'image/webp', ...]`` | Массив строк с допустимыми форматами файлов                                                          |
| ``innerText``       | String  | 'Drop file here or' | Любая строка                                                    | Текст внутри drop области компонента                                                                 |
| ``actionInnerText`` | String  |    'Browse file'    | Любая строка                                                    | Текст внутри drop области компонента, по клику на который открывается окно выбора файла для загрузки |
| `loadingText`       | String  |   `'Loading...'`    | Любая строка                                                    | Текст во время загрузки файла                                                                        |
| `tryAgainText`      | String  |    `'Try again'`    | Любая строка                                                    | Текст внутри компонента "Повторить загрузку"                                                         |
| `errorText`         | String  |        `''`         | Любая строка                                                    | Текст во время загрузки файла                                                                        |

2. объект `styleConfig`:
Набор стилей для кастомизация File Uploader.

### События
| Название         |  Тип параметров  | Описание                                        |
|------------------|:----------------:|-------------------------------------------------|
| ``error``        |      String      | Возвращает ошибку валидаторов                   |
| ``error:upload`` | [Object, String] | Возвращает ошибку, возникшую при загрузке файла |
| ``error:delete`` | [Object, String] | Возвращает ошибку, возникшую при удалении файла |
| ``on:upload``    |      String      | Возвращает id файла, который был загружен       |
| ``on:delete``    |      String      | Возвращает id файла, который был удален         |

#### Пример styleConfig
````javascript
styleConfig = {
        fontFamily: 'Open Sans',
        fileBackgroundColor: 'gray',
        fileBorderRadius: '6px',
        fileIconColor: 'black',
        fileNameFontWeight: 600,
        fileNameFontSize: '12px',
        fileNameColor: 'black',
        fileSizeFontWeight: 'gray',
        fileSizeColor: '10px',
        labelColor: 'gray',
        labelFontWeight: 500,
        labelFontSize: '12px',
        helperTextColor: 'gray',
        helperTextFontWeight: 500,
        helperTextFontSize: '10px',
        borderColor: 'gray',
        borderRadius: '6px',
        iconColor: 'gray',
        dropLabelColor: 'gray',
        browseLabelColor: 'blue',
        uploaderLabelsFontWeight: 500,
        uploderLabelsFontSize: '12px',
      }
````

## Пример использования
````vue
<template>
   <div>
      <EFileUploader
              @on:upload="uploadFile"
              @error:upload="errorUploadFile"
              @on:delete="deleteFile"
              @error="onError"
              :data="{
        label: 'Изображение',
        helperText: 'Формат png, jpeg, до 4Мб',
        maxFiles: 2,
        maxSize: 0,
        multiple: true,
        size: 'lg',
        accept: ['image/png', 'image/jpeg'],
        validators: [],
        disabled: false,
        deletable: true,
        domain: 'http://127.0.0.1:88',
        microservice: 'core',
        model: 'Image',
        innerText: 'Выберите файл или',
        actionInnerText: 'Загрузите файл',
        errorText: error,
        modelValue: files,
      }"
      />
      <hr />
   </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import EFileUploader from '@/components/inputs/FileUploader/EFileUploader.vue'
export default defineComponent({
   name: 'App',
   components: { EFileUploader },
   data() {
      return {
         files: [],
         error: '',
      }
   },
   mounted() {},
   methods: {
      errorUploadFile(er) {
         this.error = er?.message || ''
      },
      // После успешной загрузки файла - нужно обновить массив с файлами для modelValue
      uploadFile(file) {
         this.files.push(file)
      },
      deleteFile() {}
   },
})
</script>
````
