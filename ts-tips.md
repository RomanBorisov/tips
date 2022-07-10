# Шпаргалка по TS
Обозначения
```
PrimaryType - один из основных типов (string|number|boolean|т.д.)
AnyType - любой тип
```
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


## Additional tips
***T extends object*** - означает, что тип T должен быть только объектом (пустой тоже подойдет)
