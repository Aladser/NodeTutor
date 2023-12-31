### Асинхронная работа с fs через коллбэки в NodeJS

#### Асинхронное чтение файла
Метод ``readFile`` первым параметром принимает имя или путь к файлу, вторым параметром - кодировку, а третьим - коллбэк, который выполнится после чтения файла.

В коллбэк следует передавать два параметра. В первый параметр попадет объект с ошибкой, если она произойдет, а во второй - текст прочитанного файла. Для примера прочитаем текст какого-нибудь файла:
```
fs.readFile('readme.txt', 'utf8', function(err, data) {
	console.log(data);
});
```

#### Обработка исключительных ситуаций
Так как наш код асинхронный, то исключительные ситуации нельзя поймать через try-catch. Для обработки исключений в коллбэке предназначен первый параметр. Этот параметр будет содержать null, если исключения не случилось, или объект с ошибкой, если исключение произошло.

Допишем код коллбэка так, чтобы он обрабатывал исключительные ситуации:
```
fs.readFile('readme.txt', 'utf8', function(err, data) {
	if (!err) {
		console.log(data);
	} else {
		console.log('ошибка', err);
	}
});
```

#### Асинхронная запись файла
Асинхронная запись текста в файл выполняется аналогично:
```
fs.writeFile('readme.txt', 'text', function(err) {
	if (err) {
		console.log('ошибка');
	}
});
```

#### Асинхронное чтение и запись файла
Предположим нам нужно прочитать файл, сделать его текстом операцию и записать обратно в этот или другой файл. В этом случае запись в файл нужно будет делать в коллбэке чтения:
```
fs.readFile('readme.txt', 'utf8', function(err, data) {
	if (!err) {
		fs.writeFile('readme.txt', data + '!', function(err) {
			if (err) {
				console.log('ошибка записи файла');
			}
		});
	} else {
		console.log('ошибка чтения файла');
	}
});
```