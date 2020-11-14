# Сборка проекта на Gulp 4

## Modules
-     **gulp** - cборщик Gulp
-     **browser-sync** - cинхронизация кода с результатами в браузере
-     **del** - удаление каталогов и файлов
-     **gulp-autoprefixer** - добавляет префиксы в CSS код
-     **gulp-clean-css** - минификация и оптимизация CSS файлов
-     **gulp-concat** - объединение нескольких файлов в один
-     **gulp-imagemin**  - для сжатия изображений
-     **gulp-rename** - переименовывает файлы
-     **gulp-sass** - компиляция Sass и Scss файлов
-     **gulp-uglify** - сжатие и оптимизация Java Script кода
-     **gulp-group-css-media-queries** - для группировки медиа-запросов css
-     **gulp-babel** - для преобразования кода ECMAScript 2015+ в обратно совместимую версию JavaScript в современных и старых браузерах или средах
-     **gulp-html-beautify** - форматирование html разметки
-     **gulp-nunjucks-render** - Nunjucks шаблонизатор для генерирования html разметки


## Install
> Должен быть установлен Node.js

```npm i```

```npm install gulp-cli -g```


## Basic Usage

- Выполнить команду `gulp` (запуск таска default, который очистит каталог build и запустит таск scripts, styles и img-compress, а так же watch - отслеживает изменения в файлах html, css, sass, scss, js и в каталоге img)
- Писать свой код и наслаждаться автоматической сборкой проекта.

Something like this will compile your project:

```javascript
const gulp = require('gulp');
const concat = require('gulp-concat');
const autoprefixer = require('gulp-autoprefixer');
const cleanCSS = require('gulp-clean-css');
const uglify = require('gulp-uglify');
const del = require('del');
const browserSync = require('browser-sync').create();
const sass = require('gulp-sass');
const rename = require('gulp-rename');
const gcmq = require('gulp-group-css-media-queries');
const babel = require("gulp-babel");
const nunjucksRender = require('gulp-nunjucks-render');
const htmlbeautify = require('gulp-html-beautify');


const styleFiles = [
    './src/css/**/*.scss'
]

const scriptFiles = [
    './src/js/**/*.js'
]

const fontsFiles = [
    './src/fonts/**/*'
]

gulp.task('styles', () => {
    return gulp.src(styleFiles)
        .pipe(sass())
        .pipe(concat('style.css'))
        .pipe(autoprefixer({
            overrideBrowserslist: ['> 0.01%'],
            cascade: false
        }))
        .pipe(gcmq())
        .pipe(cleanCSS({
            level: 2
        }))
        .pipe(rename({
            suffix: '.min'
        }))
        .pipe(gulp.dest('./build/css'))
        .pipe(browserSync.stream());
});

gulp.task('fonts', () => {
    return gulp.src(fontsFiles)
        .pipe(gulp.dest('./build/fonts/'))
})

gulp.task('scripts', () => {

    return gulp.src(scriptFiles)
        .pipe(babel())
        .pipe(concat('main.js'))
        .pipe(uglify({
            toplevel: true
        }))
        .pipe(rename({
            suffix: '.min'
        }))
        .pipe(gulp.dest('./build/js'))
        .pipe(browserSync.stream());
});

gulp.task('del', () => {
    return del(['build/*'])
});

gulp.task('img-compress', () => {
    return gulp.src('./src/img/**')
        .pipe(gulp.dest('./build/img/'))
});

gulp.task('html', () => {
    return gulp.src('./src/pages/*.njk')
        .pipe(nunjucksRender())
        .pipe(htmlbeautify())
        .pipe(gulp.dest('./build/'));
})

gulp.task('watch', () => {
    browserSync.init({
        server: {
            baseDir: "./build"
        }
    });
    gulp.watch(['./src/pages/**/*.njk', './src/templates/**/*.njk'], gulp.series('html'))
    gulp.watch('./src/css/**/*.scss', gulp.series('styles'))
    gulp.watch('./src/js/**/*.js', gulp.series('scripts'))
    gulp.watch('./src/img/**', gulp.series('img-compress'))
    gulp.watch('./fonts/**', gulp.series('fonts'))
    gulp.watch("./build/*.html").on('change', browserSync.reload);
});


gulp.task('default', gulp.series('del', gulp.parallel('html', 'styles', 'scripts', 'img-compress', 'fonts'), 'watch'));
```