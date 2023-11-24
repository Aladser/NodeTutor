## Асинхронная работа с fs через async-await в NodeJS
Давайте теперь вместо then будем использовать альтернативный синтаксис ``async-await``.
Давайте сразу смотреть на примерах. Прочитаем текст файла и выведем его в консоль:

```
async function func() {
	let data = await fs.promises.readFile('readme.txt', 'utf8');
	console.log(data);
}
func();
```

Добавим обработку ошибок:

```
async function func() {
	try {
		let data = await fs.promises.readFile('readme.txt', 'utf8');
		console.log(data);
	} catch (err) {
		console.log('что-то пошло не так');
	}
}
func();
```

Прочитаем три файла, сольем их текст и выведем в консоль:

```
async function func() {
	try {
		let data1 = await fs.promises.readFile('1.txt', 'utf8');
		let data2 = await fs.promises.readFile('2.txt', 'utf8');
		let data3 = await fs.promises.readFile('3.txt', 'utf8');
		
		console.log(data1 + data2 + data3);
	} catch (err) {
		console.log('что-то пошло не так');
	}
}
func();
```

Запишем текст трех файлов в новый файл:

```
async function func() {
	try {
		let data1 = await fs.promises.readFile('1.txt', 'utf8');
		let data2 = await fs.promises.readFile('2.txt', 'utf8');
		let data3 = await fs.promises.readFile('3.txt', 'utf8');
		
		await fs.promises.writeFile('res.txt', data1 + data2 + data3);
	} catch (err) {
		console.log('что-то пошло не так');
	}
}
func();
```

Пусть имена наших файлов записаны в массиве. Давайте прочитаем данные наших файлов в цикле, а затем запишем их в новый файл:

```
async function func() {
	try {
		let names = ['1.txt', '2.txt', '3.txt'];
		let data  = [];
		
		for (let name of names) {
			data.push(await fs.promises.readFile(name, 'utf8'));
		}
		
		await fs.promises.writeFile('res.txt', data.join(''));
	} catch (err) {
		console.log('что-то пошло не так');
	}
}

func();
```