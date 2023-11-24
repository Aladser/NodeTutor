По умолчанию NodeJS использует модули по стандарту CommonJS.

Чтобы перейти на модули ES вам нужно разместить в папке с запускаемым вами файлом файл package.json со следующим содержимым:
```
{
	"type": "module"
}
```

Давайте для примера сделаем модуль math для математических операций. Разместим его код в файле math.js:

```
function square(num) {
	return num * num;
}
function cube(num) {
	return num * num * num;
}
```

Выполним экспорт наших функций:

```
export function square(num) {
	return num * num;
}
export function cube(num) {
	return num * num * num;
}
```
Импортируем теперь этот модуль в файл index.js:

```
import { square, cube } from './math.js';
```
Воспользуемся функциями нашего модуля:

```
let res  = square(2) + cube(3);
console.log(res);
```

### Встроенные модули
Аналогичным образом импортируются встроенные модули. Например, импортируем модуль fs для работы с файловой системой:

``import fs from 'fs';``

Необязательно импортировать все функции модуля. Можно импортировать только нужные нам:

``import { open, read, close } from 'fs';``

Подключение установленного через npm
Аналогичным образом импортируются модули, установленные через npm. Давайте для примера установим библиотеку underscore:

``npm install underscore``

Импортируем ее:

``import _ from 'underscore';``