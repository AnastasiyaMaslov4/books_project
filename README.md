# books

Проект по поиску книг с использованием Google Books API. 
Поиск происходит по ключевым словам в заголовке книги. 
Книги отображаются списком. У каждой книги указана обложка (при ее отсутствии - изображение-заглушка), авторы и категория. Если категорий несколько отображается первая. Авторы отображаются все.
Поиск запускается нажатием кнопки с лупой либо при нажатии enter. 
Результаты можно отсортировать по категориям, а также по релевантности.
При клике на "Load more" загружаются следующие 30 результатов из списка.

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
