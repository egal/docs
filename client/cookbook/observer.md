## Пример использования Observer

````javascript
    let countryObserver = new EventObserver();
    countryObserver.subscribe('Country', (data) => {
    this.$root.$emit('country-event', data);
});
````
