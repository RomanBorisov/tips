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
Переменная ***a*** имеет следующий тип - *объект состоящий из произвольного количества свойств, где название свойства должно соответствовать названию свойства из типа ```SomeType```, тип свойства такой же как и в типе ```SomeType```*
> **Пример:**  let a = { propA: 'string', propC: boolean }
>
> **Некорректный пример:**  let a = {propA: number}
