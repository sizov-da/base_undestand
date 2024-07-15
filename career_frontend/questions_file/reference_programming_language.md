### Вопросы и ответы по JavaScript

[career_questions](..%2Fcareer_questions.md)

JavaScript называют ссылочным языком из-за способа, которым он обрабатывает объекты и массивы. В JavaScript переменные могут содержать два типа данных: примитивные и ссылочные.


### Примитивные типы данных

Примитивные типы данных в JavaScript включают:
- `number`
- `string`
- `boolean`
- `null`
- `undefined`
- `symbol`
- `bigint`

Когда вы присваиваете примитивное значение переменной, это значение непосредственно сохраняется в переменной. Примитивные типы данных передаются по значению. Это означает, что при копировании переменной с примитивным типом создается новая копия значения.

**Пример:**

```javascript
let a = 5;
let b = a; // b также равно 5

b = 10; // Изменение b не влияет на a
console.log(a); // Выведет: 5
console.log(b); // Выведет: 10
```

### Ссылочные типы данных

Ссылочные типы данных включают:
- Объекты
- Массивы
- Функции

Когда вы присваиваете ссылочный тип данных переменной, в переменной сохраняется ссылка на место в памяти, где хранится сам объект. Ссылочные типы данных передаются по ссылке. Это означает, что при копировании переменной с ссылочным типом копируется ссылка на объект, а не сам объект.

**Пример:**

```javascript
let obj1 = { name: 'Alice' };
let obj2 = obj1; // obj2 теперь ссылается на тот же объект, что и obj1

obj2.name = 'Bob'; // Изменение через obj2 влияет на obj1
console.log(obj1.name); // Выведет: Bob
console.log(obj2.name); // Выведет: Bob
```

### Почему это важно:

1. **Изменяемость данных:**
    - Изменения, внесенные через одну переменную, будут видны и через другую переменную, если обе переменные ссылаются на один и тот же объект.

2. **Эффективность памяти:**
    - Вместо копирования больших структур данных (например, объектов или массивов), JavaScript копирует только ссылки на эти структуры, что экономит память.

3. **Функциональное программирование:**
    - Ссылочные типы данных позволяют передавать и возвращать сложные структуры данных из функций без значительных накладных расходов на копирование.

### Пример работы со ссылками:

**Работа с массивами:**

```javascript
let arr1 = [1, 2, 3];
let arr2 = arr1; // arr2 теперь ссылается на тот же массив, что и arr1

arr2.push(4); // Изменение через arr2 влияет на arr1
console.log(arr1); // Выведет: [1, 2, 3, 4]
console.log(arr2); // Выведет: [1, 2, 3, 4]
```

**Функции и ссылки:**

```javascript
function changeName(person) {
  person.name = 'Charlie';
}

let person = { name: 'Alice' };
changeName(person); // Передача объекта по ссылке
console.log(person.name); // Выведет: Charlie
```

### Заключение:

JavaScript называют ссылочным языком, потому что объекты и массивы в нем передаются по ссылке. Это поведение отличается от передачи примитивных типов данных по значению. Понимание этого различия помогает разработчикам предсказывать и контролировать изменения в данных при работе с переменными и функциями.