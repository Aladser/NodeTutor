### Проверка существования файла в NodeJS

Проверим существование файла или папки через метод fs.access, проверяющий возможность доступа к файлу.

Импортируем асинхронную часть модуля fs:

``import fs from 'fs/promises';``

Теперь из общей части модуля fs импортируем специальные константы:

``import { constants } from 'fs';``

После этого в переменной ``constants`` будет специальный объект с константами, содержащими специальные флаги, которые мы будем передавать параметром в метод access, изменяя его поведение.

Следующий флаг используется для проверки существования файла:

``constants.F_OK``

Давайте с помощью этого флага проверим существование файла test.txt:

``fs.access('test.txt', constants.F_OK);``

Наш метод access своим резульатом возвращает промис. Если файл существует, но промис зарезолвится, а если нет - то зареджектится:

```
fs.access('test.txt', constants.F_OK).then(() => {
	console.log('file exists');
}).catch(() => {
	console.error('file does not exists');
});
```

Давайте перепишем наш код через await. Для этого обернем вызов access в конструкцию try-catch:

```
try {
	await fs.access('test.txt', constants.F_OK);
	console.log('file exists');
} catch {
	console.error('file does not exists');
}
```

#### Другие флаги
В наш метод access можно передавать и другие флаги. К примеру, следующий флаг используется для проверки разрешения на чтение файла:

``constants.R_OK``

А следующий флаг используется для проверки разрешения на запись файла:

``constants.W_OK``

Давайте выполним проверку файла на возможность чтения:

```
try {
	await fs.access('test.txt', constants.R_OK);
	console.log('can read');
} catch {
	console.error('cannot read');
}
```

А теперь выполним проверку файла на возможность записи:

```
try {
	await fs.access('test.txt', constants.W_OK);
	console.log('can write');
} catch {
	console.error('cannot write');
}
```

А теперь проверим файл и на то, и на другое:

```
try {
	await fs.access('test.txt', constants.R_OK | constants.W_OK);
	console.log('can access');
} catch {
	console.error('cannot access');
}
```