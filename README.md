# Сборка проекта на Gulp 4

## Modules
-  **gulp** - cборщик Gulp
-  **browser-sync** - cинхронизация кода с результатами в браузере
-  **del** - удаление каталогов и файлов
-  **gulp-autoprefixer** - добавляет префиксы в CSS код
-  **gulp-clean-css** - минификация и оптимизация CSS файлов
-  **gulp-concat** - объединение нескольких файлов в один
-  **gulp-imagemin**  - для сжатия изображений
-  **gulp-rename** - переименовывает файлы
-  **gulp-sass** - компиляция Sass и Scss файлов
-  **gulp-uglify** - сжатие и оптимизация Java Script кода
-  **gulp-group-css-media-queries** - для группировки медиа-запросов css
-  **gulp-babel** - для преобразования кода ECMAScript 2015+ в обратно совместимую версию JavaScript в современных и старых браузерах или средах
-  **gulp-html-beautify** - форматирование html разметки
-  **gulp-nunjucks-render** - Nunjucks шаблонизатор для генерирования html разметки


## Install
> Должен быть установлен Node.js

```npm i```

```npm install gulp-cli -g```


## Basic Usage

- Выполнить команду `gulp` (запуск таска default, который очистит каталог build и запустит таск scripts, styles и img-compress, а так же watch - отслеживает изменения в файлах html, css, sass, scss, js и в каталоге img)
- Писать свой код и наслаждаться автоматической сборкой проекта.