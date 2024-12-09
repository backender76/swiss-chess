# Swiss Chess

[Швейцарская_система (wikipedia)](https://ru.wikipedia.org/wiki/Швейцарская_система)

[Швейцарская_система (ruwiki)](https://ru.ruwiki.ru/wiki/Швейцарская_система)

## Почему Electron

У HTML-страничек главная проблема - это отсутствие нормального доступа к файловой системе. По сути есть только localStorage, который может быть в любой момент очищен по желанию браузера. Кроме того переменные в localStorage могут пересекаться с другими такими же страничками на localhost.

Есть еще одна проблема состоит в том, что ни когда не знаешь под каким браузером придётся работать.

В итоге хотелось найти какой-то кроссплатформенный вариант. Чтобы можно было разрабатывать под linux или macOS и потом собрать билд и под windows. Рассматривались варианты Go + Qt и Go + Fyne. Но Go это совсем новый язык и там не то что-бы легко делать UI не смотря на наличие Qt и Fyne. Есть ощущение что Go не для этого изначально задумывался.

Вместо рассказа про выбор Electron будет этот пост на хабре от VK: [От Web до Desktop за 2 недели: технология Electron на практике](https://habr.com/ru/companies/vk/articles/689980/). Моя мотивация была примерно та же.

## Разработка

### Установка GIT

Не обязательно GUI, достаточно возможности выполнять консольные команды. Страницы которые могут помочь:

- https://github.com/git-guides/install-git
- https://gitforwindows.org
- https://git-scm.com

### Созаём новый репозиторий

Этот этап описан просто для того чтобы было понимание что и как делалось. **ПОВТОРЯТЬ ЕГО НЕ НУЖНО**.

Сначала локально:

```bash
# Переход в папку проекта
cd path/to/project

# Инициализация нового репозитория
git init

# Далее был создан этот README.md

# Чтобы выбрать все файлы для коммита
# Или можно было git add README.md т.к. файл один
git add .

# Сохраняем...
git commit -m "Первый коммит"
```

Теперь идём на https://github.com, создаём там новый репозиторий.

На самом деле я сделал commit немного позже, иначе этого текста тут не было бы =)

Когда репозиторий на github.com создан можно отправить туда код проекта.

```bash
git remote add origin git@github.com:backender76/swiss-chess.git
git push -u origin master
```
