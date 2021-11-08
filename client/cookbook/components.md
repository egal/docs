# Пример инициализации модели и вызова ее методов в компоненте:

1. В `mounted()` инициализируем модель и устанавливаем слушатель для
   получения данных:

```javascript
import { EgalConstructor } from "@egalteam/framework";

data() {
    return {
       exampleParams: {
          modelName: "exampleModelName",
          userName: process.env.VUE_APP_USERNAME,
          password: process.env.VUE_APP_PASSWORD,
          url: process.env.API_BASE_URL,
          connectionType: "axios",
          tokenName: "mandate"
       },
       lessonModel: null
    }
},

mounted() {
   this.lessonModel = new EgalConstructor(exampleParams)
   this.lessonModel.initModelObserver().then((data) => {
      // массив data включает в себя всю перечисленную выше информацию
   })
}

```

2. Запрос к серверу:

    ```javascript
        methods: {
            getSpeakerLessons() { // получаем данные о существующих уроках
                let filter = [ // указываем фильтры
                    {
                        field: 'speaker_id',
                        operator: 'eq',
                        value: this.$store.state.userData.id
                    }
                ];
                let withs = ['speaker', 'school', 'messages']; // указываем with
                this.lessonModel.actionGetItems(
                    'monolit', // название микросервиса
                    'axios', // способ соединения
                    undefined, // кол-во записей на страницу (если не нужно, указать undefined)
                    undefined, // определенная страница (если не нужно, указать undefined)
                    filter, // передаем фильтры
                    withs // передаем with
                );
            }
            
            acceptInvite() { // пример update запроса
                let dataForUpdate = {
                    id: this.$store.state.speakerLessonInfo.id,
                    stage: 'agreed'
                };
                this.lessonModel.actionUpdate('monolit', 'axios', dataForUpdate);
            }
        }
    ```

