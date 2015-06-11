# dancebooks-bibtex

Данный проект ставит целью собрание наиболее полной библиографии по историческим танцам. База данных хранится в формате `.bib` и предполагает интеграцию с пакетами обработки языка разметки LaTeX.

Обработка библиографической базы данных в LaTeX выполняется двумя программами: фронтэндом (на стороне LaTeX) и бэкэндом (именно бэкэнд считывает базу данных и преобразует её в формат, понятный LaTeX'у). Теоретически, можно использовать любую существующую связку (фронтэнд + бэкэнд), официально поддерживается `(biblatex + biber)`. Кроме базы данных, реализованы собственные стилевые файлы.

Для удобства пользования базой данных запущен [специальный сайт](https://bib.hda.org.ru/bib).

Кроме этого в проект включены некоторые транскрипции танцевальных источников в формате [markdown](http://daringfireball.net/projects/markdown/syntax). Некоторые правила оформления данных источников описаны ниже.

## Использование в LaTeX

Нужно установить `biblatex-2.9`, `biblatex-gost-0.9` и `biber-1.9` (про установку данных пакетов можно будет прочесть ниже. Стилевой файл подключается так:

	\newcommand{\rootfolder}{%folderpath%}
	\usepackage[root=\rootfolder]{\rootfolder/dancebooks-biblatex}

Опционально доступен параметр `usedefaults`, принимающий значения `true` и `false`. При указании значения `false` источники танцевальной библиографии (`.bib`-файлы из состава проекта) не подключаются. Позволяет использовать стилевой файл из дистрибутива в других проектах. Пример использования опции:

	\newcommand{\rootfolder}{%folderpath%}
	\usepackage[usedefaults=false,root=\rootfolder]{\rootfolder/dancebooks-biblatex}

После подключения становятся доступны макросы цитирования `\cite`, `\footcite`, `\parencite`, `\nocite`, `\volcite`. Работа макросов описана в [руководстве по biblatex](http://mirrors.ctan.org/macros/latex/contrib/biblatex/doc/biblatex.pdf). Стандартный макрос печати библиографии в `biblatex` -- `\printbibliography`, без параметров. Дополнительные библиографические источники можно добавить стандартной командой `\addbibresource{hello.bib}` (расширение `.bib` необходимо указывать явно).

Порядок компиляции такой:

1. `pdflatex project.tex`
2.	`biber --listsep=\| --namesep=\| --quiet project` (в POSIX окружении)

	`biber "--listsep=|" "--namesep=|" "test-biblatex"` (в Windows)
3. `pdflatex project.tex`
4. `pdflatex project.tex`

В версии 1.9 была добавлена поддержка `lualatex` (в качестве лингвистического фреймворка используется `Πολυγλωσσια` (`Polyglossia`)). Порядок компиляции аналогичен:

1. `lualatex project.tex`
2.	`biber '--listsep=|' '--namesep=|' '--xsvsep=\s*\|\s*' --mssplit=# project` (в POSIX окружении)

	`biber "--listsep=|" "--namesep=|" "test-biblatex" "--xsvsep=\s*\|\s*" "--mssplit=#" project` (в Windows)
3. `lualatex project.tex`
4. `lualatex project.tex`

Пункт №4 может быть опущен в случае документов без оглавления.

### Установка дополнительных пакетов. `biblatex`

Скачать можно [по этому адресу](http://sourceforge.net/projects/biblatex/files/), вот [прямая ссылка на последнюю версию](http://sourceforge.net/projects/biblatex/files/latest/download).

Пакет есть в стандартном репозитории.

### Установка дополнительных пакетов. `biber`

Скачать бэкэнд можно [по этому адресу](http://sourceforge.net/projects/biblatex-biber/files/biblatex-biber/), вот [прямая ссылка на последнюю версию](http://sourceforge.net/projects/biblatex-biber/files/latest/download). Внимание! Не всякая версия biber подходит для конкретной версии biblatex. Изучите, пожалуйста, информацию о необходимой вам версии biber.

Необходимо положить исполняемый файл в любую папку, после чего добавить эту папку в `%PATH%`, если этого не было сделано раньше.

Для x86-дистрибутивов пакет есть в стандартном репозитории (для MiKTeX он называется `miktex-biber-bin`).

### Установка дополнительных пакетов. `biblatex-gost`

Скачать последнюю версию стилей для стандарта `ГОСТ 7.0.5-2008` можно [по этому адресу](http://sourceforge.net/projects/biblatexgost/files/), вот [прямая ссылка на последнюю версию](http://sourceforge.net/projects/biblatexgost/files/latest/download).

Скачанный архив нужно распаковать (с сохранением структуры директорий) в любую из корневых папок вашего дистрибутива.

После установки пакета нужно выполнить команду обновления кэша (команда зависит от вашего дистрибутива, для MiKTeX – `initexmf -u`).

## Информация о структуре базы данных

Согласно соглашениям, наследованным от библиографических стилей `ГОСТ 7.0.5-2008` имя автора и другие метаданные указываются на языке, локальном к месту издания книги на момент её издания (метаданные русскоязычных книг переводятся в современную орфографию).

### Поддержка множества ссылок

Библиография поддерживает множество ссылок на одну и ту же книгу. Правила расстановки таковы:

1. Если книга не выложена в свободный доступ, то ссылка отсутствует.
2. Если книга доступна на ресурсах-агрегаторах ([archive.org](https://archive.org), [Google Books](https://books.google.com), [HathiTrust](http://www.hathitrust.org)), ставится ссылка на ресурс-агрегатор. Таких ссылок может быть не больше одной.
3. Если книга лежит на официальном сайте какой-либо библиотеки. Таких ссылок может быть больше, чем одна.
4. Если ссылки из пункта 3 не имеют известных дефектов, такие ссылки вытесняют ссылку из пункта 2. Информация о дефектах заносится в поле annotation.
5. Максимальное число копий одной книги не может превышать 3.

### Поддержка множества имён

Библиография поддерживает множество имён для одного и того же автора. Этот функционал предназначен для решения следующих проблем:

1. Используя оригинальное имя автора становится возможным поиск переводов его книг (за это отвечает поле `origauthor`).
2. Становится возможным поиск автора по его псевдониму (за это отвечает поле `pseudo_author`).
3. Становится возможным поиск автора по версии имени, которая указана на титульной странице книги или по той версии, которая сложилась в научной традиции (за это отвечает поле `altauthor`). `altauthor` прописывается только в том случае, когда известно настоящее имя автора.

В имени файла (поле `filename`) при этом может быть указано любое из этих имён.

### Особенности проставления поля `publisher`

Исторически сложилось так, что в поле `publisher` может быть указан издатель (человек, оплативший публикацию книги, но при этом не являющийся её единоличными автором). Издателя можно определить по наличию ключевых слов `herausgegeben von`, `printed for` и так далее.

Если издатель неизвестен, в поле `publisher` может быть указана типография или имя печатника, занимавшаяся печатью книги. Такие поля определяются словами `druckt`, `printed at`, printed by`, `imprimé par` и так далее.

Широко распространённой является практика, при которой на титульном листе книги указывается не издатель, а книгопродавец. Ввиду невозможности доказать причастность книгопродавца к изданию книги, данная информация не включается в библиографию.

### Особенности датировки

В поле `date` проставляется датировка конкретного экземпляра книги. В случае с нотными изданиями, тираж которых мог допечатываться без изменения содержания, но с изменениями титульной страницы, также ставится датировка конкретного экземпляра книги.

При невозможности установить датировку экземпляра проставляется дата цензорской отметки.

Не всякое издание можно датировать точно. Зачастую с уверенностью можно назвать лишь столетие или другой доверительный интервал. Ситуации с невозможностью точной датировки отражаются в библиографии следующим образом:

* [`1701`](https://bib.hda.org.ru/bib/books/feuillet_1701_choregraphie) — год известен точно (например, он стоит на титульном листе, в датировке цензора или в копирайте),
* [`1890?`](https://bib.hda.org.ru/bib/books/lorenzova_1890) — год известен, но никаких явных доказательств в пользу такой датировки нет (например, это датировка библиотеки или одного из множества каталогов),
* [`1803–1804`](https://bib.hda.org.ru/bib/books/noverre_1803) — год известен точно, однако издание (например, многотомная книга) выходило на протяжении нескольких лет,
* [`1700–1705?`](https://bib.hda.org.ru/bib/books/beauchamp_1700) — год известен приблизительно, в поле `annotation` при этом хранится информация о том, почему была выбрана та или иная датировка.

## О транскрипциях

Все транскрипции хранятся в формате [`markdown`](http://daringfireball.net/projects/markdown/syntax), дополнительно включён синтаксис footnotes в [http://pythonhosted.org/Markdown/extensions/footnotes.html](варианте, предлагаемом Python Markdown).

Файлы транскрипций находятся в кодировке UTF-8 (without BOM), используются UNIX-like окончания строк.

Транскрипции доступны для просмотра в веб-интерфейсе библиографии.

### Правила, применяемые при оформлении транскрипций:

Вот короткий список изменений, которые я провожу с текстом транскрипции:

* удаляются номера страниц и символы переноса внутри слов,
* \<, &lt; и \>, &gt; заменяются на ‹ и › соответственно,
* в словах (по возможности) ставится буква Ё вместо Е там, где это необходимо,
* инициалы пишутся через пробел,
* стихи и отрывки из произведений оформляются как цитаты (> ),
* короткие (однострочные) сноски вносятся прямо в текст, остальные ограничиваются горизонтальными линиями сверху и снизу (\*\*\*),
* в конце заголовков удаляются точки, в конце абзацев — наоборот, добавляются,
* в конце строк обрезаются пробельные символы,
* неочевидная расшифровка аббревиатур и опечаток помещается в круглые скобки,
* транскрипции книг, выпущенных после 1800 года переводятся в современные орфографии.

Возможны и некоторые другие специфичные для каждой транскрипции в отдельности изменения.

## О тегах

Полный список тегов с пояснениями доступен на [сайте библиографии](https://bib.hda.org.ru/bib/keywords.html).
