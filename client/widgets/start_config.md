## Генерируемые виджеты Egal

Egal предоставляет набор готовых к использованию виджетов.

Для того чтобы начать использовать библиотеку виджетов нужно:

1. установить npm пакет:

````javascript
npm i @egalteam/widget-library
````

2. импортировать библиотеку в проект:

main.js
````javascript
import egalWidgets from 'egal-widgets/dist/egal-widgets-build.umd.js'
import 'egal-widgets/dist/style.css'

app.use(egalWidgets)
````

После этого любой виджет из библиотеки можно использовать в `template` нужного компонента:

```vue
<e-avatar :data="avatarData" class="avatar"></e-avatar>
<e-button :data="buttonData"></e-button>
```