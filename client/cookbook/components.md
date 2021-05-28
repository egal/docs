## Пример инициализации модели и вызова ее методов в компоненте:

1) В `mounted()` инициализируем модель и устанавливаем слушателя для получения данных:

````javascript
import {Model} from '@egal/model/compile/index';

mounted() {
    this.lessonModel = new Model( // инициализируем модель Lesson
        'Lesson',
        process.env.VUE_APP_USERNAME,
        process.env.VUE_APP_PASSWORD
    );
    
    this.$root.$on('new-lesson-event', (data) => {
        // получение данных из Observer
    });
}
````

2) Запрос к серверу:

````javascript
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
                'getItems', // название action
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
            this.lessonRequestModel.actionUpdate('monolit', 'update', 'axios', dataForUpdate);
        }
    }
````
