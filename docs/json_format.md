---
title: "Формат JSON"
keywords: [JSON]
---

**JSON** (JavaScript Object Notation) — простой формат обмена данными, удобный для чтения и написания как человеком, так и компьютером. Он основан на подмножестве языка программирования JavaScript, опредёленного в стандарте ECMA-262 3rd Edition — December 1999. JSON — текстовый формат, полностью независимый от языка реализации, но он использует соглашения, знакомые программистам C-подобных языков, таких как C, C++, C#, Java, JavaScript, Perl, Python и многих других. Эти свойства делают JSON идеальным языком обмена данными. Файл JSON содержит только текст и использует расширение `.json`.

Есть два основных элемента объекта JSON: **ключи** и **значения**.

**Ключи** — должны быть строками. Они содержат последовательность символов, которые заключены в кавычки.

**Значения** — являются допустимым типом данных JSON. Они могут быть в форме массива, объекта, строки, логического значения, числа или значения `null`.

**Объект** — неупорядоченный набор пар ключ/значение. Объект начинается с открывающей фигурной скобки `{` и заканчивается закрывающей фигурной скобкой `}`. Каждый ключ сопровождается двоеточием. Пары ключ/значение разделяются, запятой.

```json
{"firstName":"Tom", "lastName":"Jackson", "gender":"male"}
```

```json
{
    "firstName": "Tom",
    "lastName": "Jackson",
    "gender": "male"
}
```

```json
{"employees": {"firstName":"Tom", "lastName":"Jackson"}}
```

```json
{
    "employees": {
        "firstName": "Tom",
        "lastName": "Jackson"
    }
}
```

**Массив** — упорядоченная коллекция значений. Массив начинается с открывающей квадратной скобки `[` заканчивается закрывающей квадратной скобкой `]`. Значения разделены, запятой.

Значение массива может содержать объекты JSON, что означает, что он использует ту же концепцию пар ключ/значение.

```json
[{"firstName":"Tom", "lastName":"Jackson"},{"firstName":"Linda", "lastName":"Garner"}]
```

```json
[
    {
        "firstName": "Tom",
        "lastName": "Jackson"
    },
    {
        "firstName": "Linda",
        "lastName": "Garner"
    }
]
```

```json
{"firstName":"Tom", "lastName":"Jackson", "gender":"male", "hobby":["football", "reading", "swimming"]}
```

```json
{
    "firstName": "Tom",
    "lastName": "Jackson",
    "gender": "male",
    "hobby": [
        "football",
        "reading",
        "swimming"
    ]
}
```

**Строка** — заданная последовательность из нуля и больше символов Юникода, заключённая в двойные кавычки.

```json
{"firstName":"Tom"}
```

**Число** — должно быть целым или с плавающей запятой (десятичная система счисления).

```json
{"age":30}
```

**Булевый тип** — в качестве значения используется `true` или `false`.

```json
{"married":false}
```

**Отсутствие информации** — в качестве значения используется `null`.

```json
{"bloodType":null}
```

```json
{
    "firstName": "Tom",
    "age": 30,
    "married": false,
    "bloodType": null,
}
```

```json
{
    "array": [
        1,
        2,
        3
    ],
    "boolean": true,
    "color": "gold",
    "null": null,
    "number": 123,
    "object": {
        "a": "b",
        "c": "d"
    },
    "string": "Hello World"
}
```

## Ссылки

[Online JSON Viewer](http://jsonviewer.stack.hu/)
[JSON Parser](https://jsonformatter.org/json-parser)

[[json_powershell]]
