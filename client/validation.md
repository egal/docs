## Validation Constructor

С помощью конструктора валидации можно использовать встроенные в Egal стандартные правила валидации или создавать свои, уникальные.
Список существующих правил можно найти [здесь](/client/validation_rules.md)

Конструктор принимает в качестве параметров данные для валидации, правила и кастомные сообщения об ошибках (опционально).

1) Начало работы:

```javascript
import { ValidationConstructor } from './compile/index'; // Имортируйте конструктор в нужный компонент

let data = {  // Соберите данные для валидации в объект
    name: '',
    salary: '10,000.00',
    yearOfBirth: '1980'
};
let rules = { // Определите правила валидации (ключи этого объекта должны совпадать с ключами объекта data
    name: 'required|size:3|alpha', // Можно применять несколько правил, разделяя их |
    salary: ['required', 'regex:/^(?!0\\.00)\\d{1,3}(,\\d{3})*(\\.\\d\\d)?$/'], // При использовании регулярных
    yearOfBirth: ['required', 'regex:/^(19|20)[\\d]{2,2}$/'] // выражений правила помещаются в массив
};
let message = { // Для каждого правила можно указать свое сообщение об ошибке
    alpha: ':attribute should only contain letters',
    'required.name': ':attribute is required' // Если сообщение нужно изменить только для определенного поля
};

let validator = new ValidationConstructor(data, rules, message); // Создать экземпляр класса
function validateInputs() {
    validator.getAllErrorMessages('en'); // Получение всех дефолтных сообщений об ошибках для справки
    //overrideDefaultMessage позволяет переопределить дефотнное сообщение об ошибке при необходимости
    validator.overrideDefaultMessage('required', 'Whoops, :attribute field is required.');
    validator.getAllAvailableRules(); // Получение всех дефолтных правил для справки
    validator
        .validate()
        .then((data: Array<object>) => {
           // Если какие-то из полей не прошли валидацию, вы получите массив объектов с указанием
           // названия поля и тектом сообщения об ошибке
        })
        .catch((error: object) => {
        });
}
```

2) Конструктор так же позволяет определять кастомные правила для валидации:

```javascript
// createValidationRule принимает: 
// 1) объект с указанием имени нового правила
// 2) callback функцию с указанием самого правила
// 3) сообщение об ошибке
  validator.createValidationRule({
    name: 'telephone',
    callback: function (value: string) {
      return value.match(/^\d{3}-\d{3}-\d{4}$/);
    },
    message: 'The :attribute phone number is not in the format XXX-XXX-XXXX.'
  });
```

3) Объекты данных для валидации и правил могут иметь вложенную структуру:
```javascript
// nested rules
let data = {
  name: 'John',
  bio: {
    age: 28,
    education: {
      primary: 'Elementary School',
      secondary: 'Secondary School'
    }
  }
};
// Вложенность может быть оформлена так:
let rules = {
  name: 'required',
  bio: {
    age: 'min:18',
    education: {
      primary: 'string',
      secondary: 'string'
    }
  }
};
// Либо так:
let flatRules = {
  'name': 'required',
  'bio.age': 'min:18',
  'bio.education.primary': 'string',
  'bio.education.secondary': 'string'
};
```

4) Правила валидации могут применяться в зависимости от какого-либо условия:
```javascript
// conditional rules
let data = {
  age: 30,
  name: ''
};
let rules = {
  age: ['required', { in: [29, 30] }],
  name: [{ required_if: ['age', 30] }]
```