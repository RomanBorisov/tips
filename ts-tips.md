
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
## Tip #6 Infer - захват типа в условной конструкции
![](https://habrastorage.org/r/w1560/webt/ry/yt/ck/ryytckvyu57ilxvkicuumxltwn4.jpeg)
## Tip #7 Решение проблемы множественного наследования (1 cпособ)
В TS нет возможности множественного наследования.
То есть следующий код не будет работать:
```
class ClassName1 {}
class ClassName2 {}
class ClassName3 extends ClassName1, ClassName2 {} // Не будет работать
```
Для решения этой задачи используются "Миксины".
Пример:
```
class Animal {
    feed(): void {
        console.log('feed!');
    }
}

class Movable {
    speed: number = 0;
    move(): void {
        console.log('wroom wroom!');
    }
}

class Horse {}
```
Создаем вспомогательный тип. Данный тип включает в себя только классы:
```
type Constructor = new (...args: any[]) => {}
```
Миксин
```
function applyMixins(derivedCtor: Contstructor, baseCtors: Constructor[] = []) {
    baseCtors.forEach((baseCtor) => { // Проходим по всем классам родителям
        Object.getOwnPropertyNames(baseCtor.prototype).forEach((name) => { // берем свойства родителей
            derivedCtor.prototype[name] = baseCtor.prototype[name]; // записываем их в класс-потомок
        })
    })
}
```
Использование:
```
applyMixins(Horse, [Movable, Animal]);
let pony: Horse = new Horse(); 
pony.feed();
pony.move();
```
## Tip #8 Решение проблемы множественного наследования (2 cпособ)
```
class Animal {
    feed(): void {
        console.log('feed!');
    }
}

class Horse extends Animal {}

type Constructor = new (...args: any[]) => {}

function Movable<BaseType extends Constructor>(Base: BaseType) {
    return class extends Base {
        speed: number = 0;
        move(): void {
            console.log('Wrooom');
        }
    }
}

const pony = new Horse();
pony.feed();
pony.move(); // недоступно
const MovableHorse = Movable(Horse);
const movablePony = new MovableHorse();
movablePony.feed();
movablePony.move(); // доступно
```


## Additional tips
| Tip                         | Описание                                                                                 |
|-----------------------------|------------------------------------------------------------------------------------------|
| ***T extends object***      | тип T должен быть только объектом (пустой тоже подойдет)                                 |
| ***value instanseof SomeObject*** | проверка, что value является экземпляром класса SomeObj (проверяется цепочка прототипов) |
| [K in keyof P] ***-?*** : T[K] | -? - удаляет опциональность (все поля будут обязательными).
    
  
