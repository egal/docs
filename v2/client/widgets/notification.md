## ENotification

Определяется тэгом:


```vue
<e-notification></e-notification>
```

Принимает 2 параметра в качестве пропа:

1. объект data, содержимое описано в таблице ниже

### Параметры объекта data

| Параметр           |   Тип   | По умолчанию | Возможные значения                 | Описание                                                                                                                                                                                                                              |
|--------------------|:-------:|--------------|:-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `items`            |  Array  | `[]`         | Массив объектов возможных значений | Список объектов, необходимых для генерации списка уведоилений                                                                                                                                                                         |
| `readPropertyName` | String  | `'isRead'`   | Любая строка                       | Название свойства, отвечающего за "прочитанность" уведомления                                                                                                                                                                         |
| `isReadValue`      | Boolean | `true`       | `true`, `false`                    | Зависит от того, по какой логике на бэке сформирован флаг "Прочитано\Не прочитано". Если с бэка в объекте уведомления приходят данные с ключом "прочитано" - передавать `true`, "непрочитано" - `false`. Подробнее ниже в примечаниях |
| `dotsMenu`         | Boolean | `true`       | `true`, `false`                    | Определяет отображение меню с точками                                                                                                                                                                                                 |

#### Примечания
Пример для `isReadValye`:
1. С бэка приходит массив с данными вида (названия ключей могут быть другими)
````javascript
{
    id: '1'
    title: 'Title',
    unread: true
}
````
В таком случае в `isReadValye` надо передавать `false`, т.к. в данных ключ `unread` отвечает за "непрочитанность"
2. Если приходит объект вида
````javascript
{
    id: '1'
    title: 'Title',
    read: true
}
````
В `isReadValye` надо передавать `true`, т.к. ключ `read` отвечает за "прочитанност"


2. объект styleConfig: Набор стилей для кастомизации.

### Пример styleConfig (в примере указаны дефолтные значения)

```javascript
styleConfig = {
  fontFamily: 'Inter',        // Шрифт
}
```

### Пример использования

```vue
 <template>
  <div class="notification-menu">
    <div class="empty" v-if="!notifications.length">
      <!--      some empty text-->
      no data
    </div>
    <div v-else>
      <div class="menu-header">Here is your header</div>
      <TransitionGroup name="fade" class="container">
        <ENotification
            v-for="item in notifications"
            :key="item.id"
            :data="{ item: item, dotsMenu: true, readPropertyName: 'isRead', isReadValue: true }"
            :style-config="{
            fontFamily: 'Inter',
          }"
        >
          <template #dots-menu>
            <!--          пример содержимого dots-menu -->
            <ul class="dots-list">
              <li class="action" @click="openNotification(item)">Открыть</li>
              <li class="action" @click="readNotification(item)">Отметить прочитанным</li>
              <li class="action action--danger" @click="deleteNotification(item)">Удалить</li>
            </ul>
          </template>
        </ENotification>
      </TransitionGroup>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import ENotification from '@/components/personality/notifications/ENotification.vue'

export default defineComponent({
  name: 'App',
  components: { ENotification },
  data() {
    return {
      notifications: [
        {
          id: '0',
          img: '',
          title: '111',
          text: 'Нейтральное сообщение о событии, которое не несет статусного смысла и доп. текст, который также не несёт никакого смысла',
          timeAgo: 'сейчас',
          isRead: false,
        },
        {
          id: '1',
          img: '',
          title: '222',
          text: 'Нейтральное сообщение о событии, которое не несет статусного смысла и доп. текст, который также не несёт никакого смысла',
          timeAgo: '2 мин назад',
          isRead: true,
        },
        {
          id: '2',
          img: '',
          title: '333',
          text: 'Нейтральное сообщение о событии, которое не несет статусного смысла и доп. текст, который также не несёт никакого смысла',
          timeAgo: 'сейчас',
          isRead: true,
        },
        {
          id: '3',
          img: '',
          title: 'sdfsdf',
          text: 'Нейтральное сообщение о событии, которое не несет статусного смысла и доп. текст, который также не несёт никакого смысла',
          timeAgo: 'сейчас',
          isRead: true,
        },
        {
          id: '4',
          img: '',
          title: 'sdfsdf',
          text: 'Нейтральное сообщение о событии, которое не несет статусного смысла и доп. текст, который также не несёт никакого смысла',
          timeAgo: 'сейчас',
          isRead: true,
        },
      ],
    }
  },
  methods: {
    // открыть уведомление
    openNotification(notification) {
      console.log('Открыть уведомление: ', notification)
    },

    // прочитать уведомление
    readNotification(notification) {
      console.log('Прочитать уведомление: ', notification)

      this.notifications = this.notifications.map((i) => {
        if (i.id === notification.id) {
          i.isRead = true
        }
        return i
      })
    },

    // удалить уведомление
    deleteNotification(notification) {
      console.log('Удалить уведомление: ', notification)

      this.notifications = this.notifications.filter((i) => i.id !== notification.id)
    },
  },
})
</script>

<style lang="scss">
// -- transition animation start --
.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-leave-active {
  position: absolute;
}
// -- transition animation end --

.notification-menu {
  width: 424px;
  max-height: 820px;
  overflow-y: auto;
  background: #ffffff;
  box-shadow: 0px 0px 1px rgba(12, 26, 75, 0.1), 0px 10px 16px rgba(20, 37, 63, 0.06);
  border-radius: 0 0 8px 8px;
  position: absolute;
  top: 0px;
  right: 0px;
}

.menu-header {
  border-bottom: 1px solid #e2e8f0;
  background: #ffffff;
  padding: 24px;
  border-radius: 8px 8px 0 0;
}

.dots-menu {
  font-family: 'Inter';
  position: absolute;
  right: 0;
  width: 188px;
  padding: 8px 14px;
  background: #ffffff;
  box-shadow: 0px 0px 1px rgba(12, 26, 75, 0.24), 0px 3px 8px -1px rgba(50, 50, 71, 0.05);
  border-radius: 6px;
  margin-top: 6px;

  .dots-list {
    list-style: none;
    margin: 0;
    padding: 0;
    gap: 6px;
    display: flex;
    flex-direction: column;
    .action {
      padding: 6px;
      background: #ffffff;
      border-radius: 4px;
      font-weight: 400;
      font-size: 12px;
      line-height: 150%;
      display: flex;
      align-items: center;
      color: #2d3748;

      &--danger {
        color: #f16063;
      }
      &:hover {
        background: #f7fafc;
      }
    }
  }
}
</style>

```

