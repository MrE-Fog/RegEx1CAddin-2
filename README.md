[![Join the chat at https://gitter.im/RegEx1CAddin/Lobby](https://badges.gitter.im/RegEx1CAddin.svg)](https://gitter.im/RegEx1CAddin/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

# RegEx1CAddin

Внешняя Native API компонента для выполнения регулярных выражений на платформе 1С:Предприятие 8. Написана на C++. Используется движок boost::regex (v 1.69). Версия синтаксиса Perl Compatible Regular Expressions.

Внимание! Текущая версия является тестовой и еще не была протестирована на реальных проектах и базах. 
В текущей версии нет совместимости с Windows XP (совместимость была до версии 4).

Текущая версия собрана для следующих платформ:
Windows 32bit 
Windows 64bit 
Linux 32bit 
Linux 64bit 
MacOS 64bit 

Тестировалось на платформе 8.3.12.1567 (Windows 7, Windows Server 2008 R2, Ubuntu 14 32-64bit, MacOS Sierra 10.12)

Сборка осуществлялась с использованием следующих инструментов:

Под Windows: Microsoft Visual Studio Community 2017

Под Linux: GCC 6

Под Mac OS: Clang 9

Использовалась статическая сборка, поэтому компонента не требует установки каких-либо дополнительных библиотек.

Компонента реализует следующие методы:

### Метод НайтиСовпадения / Matches(<Текст для анализа>, <Регулярное выражение>).

Метод выполняет поиск в переданном тексте по переданному регулярному выражению.

Результатом выполнения метода будет массив результатов поиска. Каждый элемент массива - найденная подгруппа поиска. Если подгрупп нет, то массив будет содержать один элемент - найденную строку.

Возвращаемое значение: Ничего не возвращает.

Для того, чтобы получить результаты выполнения метода (массив результатов), необходимо выполнить метод Следующий/Next(), и после этого, в свойстве ТекущееЗначение/CurrentValue будет доступно значение текущей подгруппы результата выполнения (текущий элемент массива результатов). Идея  похожа на обход результата запроса.

Пример:

Рег.НайтиСовпадения("Hello world", "([A-Za-z]+)\s+([a-z]+)");

Пока Рег.Следующий() Цикл
    Сообщить(Рег.ТекущееЗначение);    
КонецЦикла; 
Результат будет содержать 3 элемента:

Hello world

Hello

world


### Метод Количество()/Count()

Возвращает количество результатов поиска, после выполнения метода НайтиСовпадения / Matches

### Метод Заменить/Replace(<Текст для анализа>, <Регулярное выражение>, <Значение для замены>)

Заменяет в переданном тексте часть, соответствующую регулярному выражению, значением, переданным третьим параметром.

Возвращаемое значение: Строка, результат замены.

### Метод Совпадает/IsMatch(<Текст для анализа>, <Регулярное выражение>)

Делает проверку на соответствие текста регулярному выражению.

Возвращаемое значение: Булево. Возвращает значение Истина если текст соответствует регулярному выражению.

### Метод Версия/Version()

Возвращает номер версии компоненты в виде строки.

Возвращаемое значение: Строка

### Свойство ВсеСовпадения/Global

Тип: Булево.

Значение по умолчанию: Ложь.

Если установлено в Истина, то поиск будет выполняться по всем совпадениям, а не только по первому.

### Свойство ИгнорироватьРегистр/IgnoreCase

Тип: Булево.

Значение по умолчанию: Ложь.

Если установлено в Истина, то поиск будет осуществляться без учета регистра.

### Свойство Шаблон/Template

Тип: Строка.

Значение по умолчанию: пустая строка.

Задает регулярное выражение которое будет использоваться при вызове методов компоненты, если в метод не передано значение регулярного выражения.

### Свойство ОписаниеОшибки/ErrorDescription

Тип: Строка.

Значение по умолчанию: пустая строка.

Содержит текст последней ошибки. Если ошибки не было, то пустая строка.

### Свойство ВызыватьИсключения/ThrowExceptions

Тип: Булево.

Значение по умолчанию: Ложь.

Если установлена в Истина, то при возникновении ошибки, будет вызываться исключение, при обработке исключения, текст ошибки можно получить из свойства ErrorDescription\ОписаниеОшибки.
Пример использования:

Предполагается что архив с компонентами был загружен в общий макет "RegEx"

УстановитьВнешнююКомпоненту("ОбщийМакет.RegEx");
ПодключитьВнешнююКомпоненту("ОбщийМакет.RegEx", "Component", ТипВнешнейКомпоненты.Native);
            
Рег = Новый("AddIn.Component.RegEx");
Рег.НайтиСовпадения("Hello world", "([A-Za-z]+)\s+([a-z]+)");

Пока Рег.Следующий() Цикл
    Сообщить(Рег.ТекущееЗначение);    
КонецЦикла; 

Сообщить(Рег.Количество());

Сообщить(Рег.Совпадает("Hello world", "([A-Za-z]+)\s+([a-z]+)"));

Сообщить(Рег.Заменить("Hello world", "([A-Za-z]+)\s+([a-z]+)", "Текст для замены"));
