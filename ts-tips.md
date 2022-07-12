# Шпаргалка по TS
Полезные ссылки:
- https://www.typescriptlang.org/docs/handbook/utility-types.html - utility тайпы (типа Omit и тп)
- https://olegbarabanov.github.io/google-typescript-style-guide-ru/ Руководство Google по стилю написания кода на языке TypeScript (перевод)
## Tip #1
```
let a: {
    [key: string]: AnyType
}
```
Переменная ***a*** имеет следующий тип - *объект состоящий из произвольного количества свойств, где название свойства должно иметь тип string, значение свойства может иметь любой тип.*
> **Пример:**
> ```
> let a = { 
>   someProp: 'string', 
>   lorem: { 
>       prop: 'hello' 
>   }
> };
> ```

## Tip #2
```
type SomeType = {
    propA: string;
    propB: number;
    propC: boolean;
}

let a: {
    [key in keyof SomeType]: SomeType[key]
}
```

Переменная ***a*** имеет следующий тип - *объект состоящий из произвольного количества свойств, где название свойства
должно соответствовать названию свойства из типа ```SomeType```, тип свойства такой же как и в типе ```SomeType```*
> **Пример:** ``` let a = { propA: 'string', propC: boolean }```
>
> **Некорректный пример:**  let a = {propA: number}

## Tip #3

```
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}
```

Дженерик ***K*** обязан предствлять из себя ключ дженерика T
> **Пример:**
> ``` 
> interface ISomeInterface = {
>    propA: string;
>    propB: number;
> }
> let obj: ISomeInterface = { propA: 'string', propB: 2 };
> 
> getPropert(obj, `propA`); - корректно
> getPropert<ISomeInterface, 'propA' | 'propB' >(obj, `propA`); - корректно
> 
> getPropert(obj, `variable`); - вернет ошибку
> getPropert<ISomeInterface, 'propA'>(obj, `variable`);- вернет ошибку
> 
> ```
## Tip #4
***T[keyof T]*** - получить объеденение типов свойств объекта T
>  **Пример:**
> ```
> interface ISomeInterface = {
>    propA: string;
>    propB: number;
> }
> 
> function someFunc<T>(obj: T): T[keyof T] {
>  ///
> }
> 
> someFunc<ISomeInterface>(a: {propA: 'asd', propB: 2})
> ```
> Вызов функции с данными параметрами будет возвращать тип  string | number
## Tip #5 Проверка, что переменная реализует  конкретный интерфейс
```
interface IFish {
    swim: () => void;
}
interface IBird {
    fly: () => void;
}

function isFish(pet: IFish | IBird) pet is Fish {
    return (pet as IFish).swim !== undefined   && typeof (pet as Fish).swim !== 'function;
}

```


## Additional tips
| Tip                         | Описание                                                                                 |
|-----------------------------|------------------------------------------------------------------------------------------|
| ***T extends object***      | тип T должен быть только объектом (пустой тоже подойдет)                                 |
| **_value instanseof SomeObject_** | проверка, что value является экземпляром класса SomeObj (проверяется цепочка прототипов) |
