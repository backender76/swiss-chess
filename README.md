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

Теперь идём на [github.com](https://github.com), создаём там новый репозиторий.

На самом деле я сделал commit немного позже, иначе этого текста тут не было бы =)

Когда репозиторий на github.com создан можно отправить туда код проекта.

```bash
git remote add origin git@github.com:backender76/swiss-chess.git
git push -u origin master
```

### Git flow

Настоящий [GitFlow](https://www.atlassian.com/ru/git/tutorials/comparing-workflows/gitflow-workflow) использовать не будем, но какой-то Flow должен быть. Договоримся так:

- Фичи пилим в отдельных ветках стартуя их из `develop`
- Рабочий код сливаем в ветку `develop`
- Релизы в ветке `master`, помечаем их тегами

Например, возникла идея какой-то новой мега-фичи, порядок действий должен быть такой:
- Затянуть ветку `develop` с `origin`
- От нее ответвить свою ветку `mega-feature`
- Пилим `mega-feature` периодически затягивая `develop` и выполняя от нее `rebase`
- Как фича закончена, создаём PR (pull request) в `develop`

Настало время создать develop:

```bash
# ПРОСТО ДЛЯ ПРИМЕРА. ПОВТОРЯТЬ НЕ НУЖНО Т.К. ВЕТКА УЖЕ ЕСТЬ
git checkout -b develop
git add README.md
git commit -m "Первый коммит в develop (Git flow)"
git push -u origin develop
```

### Как скопировать код проекта

При наличии Git на рабочей машине все довольно просто:

```bash
git clone git@github.com:backender76/swiss-chess.git
```

### Установка Electron

Для установки потребуется потребуется [Node.JS](https://nodejs.org/en). Я использовал v23.3.0. Можно поставить какая поставится, а потом поменять с помощью [npm n](https://www.npmjs.com/package/n).

Далее все по зветам [мануала по Electron](https://www.electronjs.org/ru/docs/latest/tutorial/quick-start):

```bash
# Electron ставился так. Этого делать 2-й раз не нужно!!!
npm init
npm install --save-dev electron

# После клонирования репозитория, нужно выполнить npm ci в папке проекта.
# Это установит все пакеты описанны в package.json и package-lock.json.
npm ci
```

После установки электрона появится папка `node_modules`. Там будут все зависимости проекта включая сам электрон. Чтобы `GIT` не пытался их сохранить в репозитории сразу создаём файл `.gitignore` и прописмываем туда `node_modules`. В репозиторий это совать не принято. Описание зависимостей будет в `package.json` и `package-lock.json`. Любой кто клонирует к себе репозиторий сможет с лёгкостью установить их командой `npm ci`.


```bash
# Основное
npm init
npm install --save-dev electron
npm install --save-dev @electron-forge/cli

# Это для Linux-a если нужны rpm-пакеты
sudo apt-get update
sudo apt-get install rpm
npm install --save-dev @electron-forge/maker-rpm

# Генерим forge.config.js
# Мне не нужен rpm, так что соответствующий блок в конфиге закомментирован
npx electron-forge import

# Далее я добавил index.html, main.js и preload.js по примеру в мануале.

# Уже можно пробовать запустить приложение
npm run start

# Создание билда
npx electron-forge make

# Установка. Чтобы сработало версия должна меняться.
sudo dpkg -i out/make/deb/x64/swiss-chess_1.0.1_amd64.deb
```
