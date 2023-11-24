### Имя папки со скриптом в NodeJS
Если ваш NodeJS работает в стиле CommonJS, то в файлах с вашими скриптами будет доступна константа __dirname:

``console.log(__dirname);``

В ES модулях, однако, эта константа была убрана. Впрочем, ее несложно получить самому. Сделаем для этого файл __dirname.js, экспортирующий нужный нам путь к папке со скриптом:

```
import { dirname } from 'path';
import { fileURLToPath } from 'url';

const __dirname = dirname(fileURLToPath(import.meta.url));
export default __dirname;
```

Теперь в нужно исполняемом файле мы можем импортировать созданный нами модуль и получить нужную нам константу:

```
import __dirname from './__dirname.js';
console.log(__dirname);
```