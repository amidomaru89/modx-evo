### author webber (web-ber12@yandex.ru)

### набор evoSearch - индексирование и поиск с учетом морфологии (версия 0.1)

### DONATE
---------
если считаете данный продукт полезным и хотите отблагодарить автора материально,
либо просто пожертвовать немного средств на развитие проекта - 
можете сделать это на любой удобный Вам электронный кошелек в системе <strong>Webmoney</strong><br>
<strong>WMR:</strong> R133161482227<br>
<strong>WMZ:</strong> Z202420836069<br>
с необязательной пометкой от кого и за что именно :)


### содержит:
- плагин evoSearch - для индексирования (в настоящий момент индексации по TV не производится, индексируются только pagetitle,longtitle,description,introtext.content)
- сниппет evoSearch - для вывода результатов поиска. В качестве "рабочей лошадки" вывода используется сниппет DocLister (но можно использовать и Ditto, указав параметр &worker=`Ditto` -  результаты поиска могут быть чуть хуже)
- идея использования phpMorphy - http://valera.ws/2007.09.05~morpho_search_in_mysql/


### обязательные дополнительные компоненты (без их установки решение не работает либо работает хуже):
- сниппет DocLister

### установка
устанавливаем плагин и сниппет. Для плагина событие onDocFormSave, 
параметры  - &offset=Первая строка переиндексации;text;0 &rowsperonce=Строк за сеанс индексировать;text;1 &reindex=Переиндексировать все;text;0 &excludeTmpls=Исключить шаблоны;text; &excludeIDs=Исключить ID ресурсов;text; &TvNames=Имена TV для поиска;text; &unpublished=Индексировать неопубликованные;text;0 &deleted=Индексировать удаленные;text;0 &dicts=Использовать словари;text;rus,eng

 * до первого запуска сниппета на фронтэнде сайта необходимо запустить индексацию (сохранить любой ресурс в админке)
 * создание необходимых полей content_with_tv и content_with_tv_index, а также нужных индексов производится автоматически (!!!) при первом запуске индексации
 * индексация запускается сохранением любого ресурса (вызовом события onDocFormSave)
 
при первом запуске или необходимости переиндексации необходимо выставить параметр "Переиндексировать все" = 1, начальные строки и количество строк за сеанс устанавливаются в зависимости от 
возможностей вашего хостинга (например 0 и 10 000 соответственно - проиндексирует строки с 0 в количестве 10 000 штук в БД
необходимо открыть и пересохранить любой документ для создания события onDocFormSave

для последующей работы установите "Переиндексировать все" = 0, "Строк за сеанс индексировать" = 1 
при этом происходит переиндксация только того документа, который сохраняется

### пример вызова
    для вывода результатов [!evoSearch? &tpl=`evoSearch`!], 

### ПАРАМЕТРЫ
 + &noResult = `Ничего не найдено` - строка, которая выводится при отсутствии результата поиска )необязательно)
 + &addSearch = `0` - для опционального отключения дополнительного поиска при пустом fulltext-search (по умолчанию - 1)
 + &extract=`0` - отключить экстректор - формирует нужную часть текста с подсветкой из результатов поиска (плейсхолдер [+extract+] в чанке вывода результатов DocLister) - по умолчанию 1 (извлекать)
 + &maxlength=`300` - максимальная длина извлекаемой части текста в резуьлтатах поиска (по умолчанию 350)
 + &show_stat = `0` - отключаем показ статистики "найдено....показано...с...по...". По умолчанию - 1 - показ включен
 + &statTpl - шаблон показа статистики (по умолчанию - <div class="evoSearch_info">По запросу <b>[+stat_request+]</b> найдено всего <b>[+stat_total+]</b>. Показано <b>[+stat_display+]</b>, c [+stat_from+] по [+stat_to+]</div> ), где
                [+stat_request+] - запрос из строки $_GET['search']
                [+stat_total+] - найдено всего
                [+stat_display+] - показано на текущей странице с [+stat_from+] по [+stat_to+] 
 + &rel = `1` - релевантность поиска, по умолчанию 2, чем выше цифра - тем более релевантные результаты и тем их меньше
 + &addLikeSearch = `1` - добавляем функционал для поиска через like (на случай, если слов не было в словаре). По умолчанию - 0
 + addLikeSearchType - тип поиска addLikeSearch (oneword- любое слово, allwords-все слова). По умолчанию exact - фраза целиком
 + addLikeSearchLength - минимальная длина слова для поиска в addLikeSearch (по умолчанию - 3)
 
остальные параметры - дублируют параметры вызова DocLister

обрабатывает $_GET['search'] в качестве входной строки для поиска 

подсветка найденных слов в pagetitle и extract в результатах поиска осуществляется тегом <span class="evoSearch_highlight"> - т.е. возможна ее стилизация через css-файлы
   
### информация. 
    Т.к. при полнотекстовом поиске MySQL без дополнительных настроект обрабатываются только слова не короче 4 символов, для улучшения результатов поиска используется дополнительный поиск 
    средствами фильтров DocLister (что улучшает результаты, особенно при их отстутствии в результате обычного поиска).
    Совместим с DocLister версии 1.4.1 и ниже, 1.4.8 и выше. В версиях 1.4.5, 1.4.6, 1.4.7 встречается некорректный сброс строки $_GET, из-за чего некорректно срабатывает обработка пустых результатов


### Сотрудничество:
---------
По вопросам сотрудничества обращайтесь на электронный ящик web-ber12@yandex.ru
