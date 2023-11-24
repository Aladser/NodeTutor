### Асинхронная работа с fs через then в NodeJS

С методами модуля fs асинхронно можно работать не только через коллбэки, но и через промисы. Для этого есть специальное свойство promises, содержащее в себе промисные аналоги методов для работы с файловой системой. К примеру, для метода fs.readFile его промисный аналог будет fs.promises.readFile.

#### Чтение файлов
Давайте выведем в консоль содержимое какого-нибудь файла:
```
fs.promises.readFile('readme.txt', 'utf8').then(data => {
	console.log(data);
});
```
#### Обработка исключений
Добавим теперь обработку исключительных ситуаций:

```
fs.promises.readFile('readme.txt', 'utf8').then(data => {
	console.log(data);
}).catch(err => {
	console.log('ошибка');
});
```

#### Чтение и запись
Можно прочитать файл, что-то сделать с его текстом, а потом записать обратно:

```
fs.promises.readFile('readme.txt', 'utf8').then(data => {
	return fs.promises.writeFile('readme.txt', data + '!');
}).catch(err => {
	console.log('ошибка');
});
```

#### Массовая работа
Пусть у нас есть несколько файлов. Давайте прочитаем эти файлы, сольем их текст в одну строку и запишем ее в новый файл.

В отличие от коллбэков, в данном случае нам нет нужды выполнять чтение файлов по очереди. При работе с промисами мы можем записать все промисы для чтения файлов в массив, а потом воспользоваться Promise.all, чтобы осуществить запись в файл только тогда, когда все файлы будут прочитаны.

Давайте сделаем это. Пусть имена файлов у нас есть в виде массива:

``let names = ['1.txt', '2.txt', '3.txt'];``

Запустим цикл, в котором будем читать файлы, записывая промисы с результатами в массив:

```
let names = ['1.txt', '2.txt', '3.txt'];
let files = [];

for (let name of names) {
	files.push(fs.promises.readFile(name, 'utf8'));
}
console.log(files); // массив промисов
```

Имея такой массив, мы можем вызвать then только один раз, когда все промисы выполнятся:

```
Promise.all(files).then(data => {
	fs.promises.writeFile('res.txt', data.join(''));
});
```

Добавим обработку исключительных ситуаций:

```
Promise.all(files).then(data => {
	fs.promises.writeFile('res.txt', data.join(''));
}).catch(err => {
	console.log('что-то пошло не так');
});
```

Соберем весь наш код вместе:

```
let names = ['1.txt', '2.txt', '3.txt'];
let files = [];

for (let name of names) {
	files.push(fs.promises.readFile(name, 'utf8'));
}

Promise.all(files).then(data => {
	fs.promises.writeFile('res.txt', data.join(''));
}).catch(err => {
	console.log('что-то пошло не так');
});
```