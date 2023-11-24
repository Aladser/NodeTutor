### Сокращение fs.promises в NodeJS

Вспомним, как мы подключали модуль fs:

``import fs from 'fs';``

После такого подключения нам будут доступны промисные варианты методов этого модуля через fs.promises:

```
fs.promises.readFile('readme.txt', 'utf8').then(data => {
	console.log(data);
});
```

Однако, писать свойство promises перед каждым нужным методом не очень удобно. Давайте поступим хитрее - запишем в переменную fs не сам модуль, а его часть с промисами:

``import fs from 'fs/promises';``

И теперь мы можем опускать раздражающее нас свойство:

```
fs.readFile('readme.txt', 'utf8').then(data => {
	console.log(data);
});
```