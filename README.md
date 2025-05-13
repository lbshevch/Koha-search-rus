## Поддержка языков  

Список поддерживаемых языков:
```
koha-translate --list --available 
```
Установка требуемого языка:
```
apt install yarn

koha-translate --install ru-RU
koha-translate --update ru-RU
```
Чтобы обеспечить корректный поиск c помощью системы индексирования и поиска Zebra на нелатинских языках (таких как русский, китайский и арабский) в Koha, необходимо настроить и сконфигурировать цепочки ICU (Международные компоненты для юникода). Читаем [Конфигурация цепочек ICU](https://wiki.koha-community.org/wiki/ICU_chains_configuration)

Чтобы включить и настроить ICU:

1.  Установите пакет yaz-icu:
```
apt install yaz-icu
```
2. В интерфейсе персонала перейдите в раздел **Дополнительно** > **Администрирование** > **Глобальные системные настройки** > **Поиск**
3. Измените системную настройку ***UseICUStyleQuotes*** на *Using* .
4. Измените системную настройку ***QueryFuzzy*** на *Don't try* .
5. Измените системную настройку ***QueryStemming*** на *Don't try* .
6. Отредактируйте /etc/koha/zebradb/etc/default.idx
Измените или добавьте строки: **icuchain words-icu.xml** и **icuchain phrases-icu.xml** 
```
 # Traditional word index
 # Used if completenss is 'incomplete field' (@attr 6=1) and
 # structure is word/phrase/word-list/free-form-text/document-text
 index w
 completeness 0
 position 1
 alwaysmatches 1
 firstinfield 1
 icuchain words-icu.xml
 
 # Phrase index
 # Used if completeness is 'complete {sub}field' (@attr 6=2, @attr 6=1)
 # and structure is word/phrase/word-list/free-form-text/document-text
 index p
 completeness 1
 firstinfield 1
 icuchain phrases-icu.xml 
```
7. Замените оригинальные файлы **words-icu.xml** и **phrases-icu.xml**, находящиеся в папке **/etc/koha/zebradb/etc/** своими файлами **[words-icu.xml](./words-icu.xml)** и **[phrases-icu.xml](phrases-icu.xml)** для русского языка.
8. Перезапустите Zebra и перестройте поисковый индекс
Если вы используете пакеты (рекомендуемый способ установки Koha), выполните:
```
koha-zebra --restart {название вашей библиотеки}
koha-rebuild-zebra -f {название вашей библиотеки}
```
