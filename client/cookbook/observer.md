## Пример использования Observer

````javascript
    let countryObserver = new EventObserver();
    countryObserver.subscribe('Country', (data) => {
        /**
         *  emit ниже - это пример реакции на событие, которое получает Observer.
         *  Вместо него может быть любой другой нужный пользователю метод.
         */
         this.$root.$emit('country-event', data);
});
````
