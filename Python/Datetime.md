---
link: "https://telegra.ph/Modul-datetime-v-Python-kak-rabotat-s-datoj-i-vremenem-12-27"
---
# [Модуль datetime в Python: как работать с датой и временем – Telegraph](https://telegra.ph/Modul-datetime-v-Python-kak-rabotat-s-datoj-i-vremenem-12-27)
@python2day  

![](https://fixmypc.ru/media/media/news_main_images/16.jpg)Работа с датой и временем в модуле Python datetime

 Для работы с датой и временем в **Python** 2 и 3 есть отдельный модуль **datetime**. В отличие от других языков, таких как SQL, в Python нет встроенного типа данных для работы с датой и временем - это достигается путем импорта модуля и созданием объекта. Модуль **datetime** входит в пакет Python и его не нужно отдельно устанавливать.

Импорт модуля осуществляется следующим путем:

```python
import datetime
```

**Навигация по посту**

* [[#Получение текущей даты и времени]]
* [[#Форматирование и перевод в строку]]
* [[#Получения дня недели и название месяца]]
* [[#Создание объекта даты и времени]]
* [[#Создание из строк]]
* [[#Продолжительность времени с timedelta]]
* [[#Разница между датами]]
* [[#Изменение объектов]]
* [[#Сравнение и условия]]
* [[#Работа с метками (штампами) timestamp]]
* [[#Работа с часовыми поясами]]

###  ###

### Получение текущей даты и времени ###

Для вывода даты и времени нужно выполнить следующее:

```python
import datetime
day_time = datetime.datetime.now()
print(day_time)
```

![](https://fixmypc.ru/media/uploads/2019/12/26/11.jpg)Получение текущей даты и времени в Python datetime

Как видно мы получаем для создания объекта даты и времени используется класс с тем же именем что и модуль **datetime**.

Когда нужно получить только дату используется класс **date**:

```python
import datetime
day = datetime.date.today()
print(day)
```

![](https://fixmypc.ru/media/uploads/2019/12/26/10.jpg)Получение текущей даты в Python datetime

Получение только времени выполняется через метод time():

```python
import datetime
date_time = datetime.datetime.now().time()
print(date_time, type(date_time))
```

![](https://fixmypc.ru/media/uploads/2019/12/28/17.jpg)Получение времени в Python datetime

Каждый из описываемых классов можно импортировать следующим способом:

```python
from datetime import date
from datetime import datetime
print(datetime.now())
print(date.today())
```

![](https://fixmypc.ru/media/uploads/2019/12/29/28.jpg)Импорт модуля datetime в Python

### Форматирование и перевод в строку ###

Для получение части даты или времени можно использовать следующие атрибуты:

* year
* month
* day
* weekday
* hour
* minute
* second

```python
import datetime
# Получение дня от даты
date = datetime.date.today()
print(date.day)
date_time = datetime.datetime.now()
# Получение часа суток от времени
print(date_time.hour)
```

![](https://fixmypc.ru/media/uploads/2019/12/26/9.jpg)Форматирование строки с датой в Python datetime

Так же есть метод **strftime**, который форматирует даты в нужном формате в строку. Например так мы получим дату в формате, который используется у нас:

```python
import datetime
date = datetime.datetime.today()
print(date.strftime('%d-%m-%Y %H:%M'))
```

![](https://fixmypc.ru/media/uploads/2019/12/26/8.jpg)Форматирование в datetime Python

Где:

* %d - день месяца с 1 по 31;
* %m - месяц с 1 по 12;
* %Y - год;
* %H - час в формате 0-24;
* %M - минуты;
* %S - секунды.

Таким же способом можно получить время и дату:

* %c - время и дата;
* %x - дата;
* %X - время.

```python
date = datetime.datetime.today()
print(date.strftime('%x'))
```

![](https://fixmypc.ru/media/uploads/2019/12/26/7.jpg)Форматирование в datetime Python

Обратите внимание, что таким способом мы преобразуем объект класса **datetime** в строку и мы больше не сможем использовать методы по работе с датой (например сравнение):

```python
import datetime
date = datetime.datetime.today()
local_date = date.strftime('%d-%m-%Y %H:%M')
print('Объект класса datetime: ', type(date))
print('Дата преобразованная в строку: ', type(local_date))
print(local_date.day)
```

![](https://fixmypc.ru/media/uploads/2019/12/26/6.jpg)Типы дат в datetime Python

Мы получим ошибку так как уже работаем со строкой:

* AttributeError: 'str' object has no attribute 'day'

Выше описаны основные возможности форматирования используя метод **strftime()**, но их, конечно, больше.

### Получения дня недели и название месяца ###

Можно получить название дня недели или название. Численный вариант эквивалентен следующим значениям:

* 0 - Monday (Понедельник);
* 1 - Tuesday (Вторник);
* 2 - Wednesday (Среда);
* 3 - Thursday (Четверг);
* 4 - Friday (Пятница);
* 5 - Saturday (Суббота);
* 6 - Sunday (Воскресенье).

Следующий пример вернет день недели в виде числа:

```python
date = datetime.datetime.today()
print(date.weekday())
```

![](https://fixmypc.ru/media/uploads/2019/12/26/12.jpg)Получение дня недели в Python datetime

Или получить название:

```python
date = datetime.datetime.today()
print(date.strftime('%A'))
print(date.strftime('%a'))
```

![](https://fixmypc.ru/media/uploads/2019/12/26/13.jpg)Получение имени дня недели в Python datetime

 Где:

* %A - полное название дня недели;
* %a - сокращенное название дня недели;
* %s - представление в виде числа.

Такой же принцип по работе с месяцами, где:

* %B - полное название месяца;
* %b - сокращенное название месяца;
* %m - месяц в виде числа.

### Создание объекта даты и времени ###

Для создания даты используется класс с аргументами date(год, месяц, число):

```python
date = datetime.date(2019, 1, 13)
print(date, type(date))
```

![](https://fixmypc.ru/media/uploads/2019/12/28/14.jpg)Создание объекта с даты в Python datetime

Можно создать дату и время. В обоих случаях мы можем использовать именованные аргументы если так удобнее:

```python
date_time = datetime.datetime(year = 2019,
                         month = 2,
                         day = 4,
                         hour = 7,
                         minute = 9,
                         second = 13 )
print(date_time, type(date_time))
```

![](https://fixmypc.ru/media/uploads/2019/12/28/15.jpg)Создание объекта с датой и временем в Python datetime

Каждый аргумент времени, по умолчанию, имеет значение 0. Мы так же можем использовать подход выше для получения, например, только года или времени:

```python
date_time = datetime.datetime(year = 2019,
                         month = 2,
                         day = 4,
                         second = 13 )
# Получаем только время
time = date_time.strftime('%X')
print(time, type(time))

date = datetime.date(
                     year = 2018,
                     month = 5,
                     day = 3
                     )
# Получаем только год
year = date.year
print(year, type(year))
```

![](https://fixmypc.ru/media/uploads/2019/12/28/16.jpg)Именованные аргумента в datetime Python

Если вы не укажете год, месяц или день, то получите ошибку т.к. они по умолчанию равны None:

* TypeError: Required argument 'year' (pos 1) not found

Есть отдельный класс для создания времени time:

```python
time = datetime.time(12, 33, second=33, microsecond=122)
print(time, type(time))
print(time.hour)
```

![](https://fixmypc.ru/media/uploads/2019/12/28/18.jpg)Создание объекта времени в datetime Python

### Создание из строк ###

Используя **strptime()** можно создавать объект **datetime** из строки:

```python
date_string = '21 September. 1999'
date_object = datetime.strptime(date_string, '%d %B. %Y')
print(date_object, type(date_object))
```

![](https://fixmypc.ru/media/uploads/2019/12/28/19.jpg)Создание даты из строки с datetime strptime Python

### Продолжительность времени с timedelta ###

**timedelta** - это класс с помощью которого можно установить не дату, как в примерах выше, а продолжительность. Так мы создадим объект с 12 днями и 33 секундами:

```python
duration_time = datetime.timedelta(days = 12, seconds = 33)
print(duration_time, type(duration_time))
```

![](https://fixmypc.ru/media/uploads/2019/12/29/20.jpg)Продолжительность с timedelta в Python datetime

Все атрибуты, которые мы можем указывать для этого класса:

* days
* seconds
* microseconds
* milliseconds
* minutes
* hours

Кроме этого мы можем преобразовывать эти объекты в секунды:

```python
duration_time = datetime.timedelta(minutes = 1, seconds = 10)
# Преобразуем в секунды
seconds = duration_time.total_seconds()
print(seconds, type(seconds))
```

![](https://fixmypc.ru/media/uploads/2019/12/29/21.jpg)Получение секунд в datetime Python

### Разница между датами ###

Мы можем искать разницу между датами получая объект **timedelta**:

```python
date_now = datetime.datetime.now()
date_tomorrow = datetime.datetime(2019, 12, 29)
# Вычисляем разницу между датами
difference = date_tomorrow - date_now
print(difference, type(difference))
print('Разница только в днях: ', difference.days)
```

![](https://fixmypc.ru/media/uploads/2019/12/29/24.jpg)Разница между датами в Python datetime

### Изменение объектов ###

Каждый их объектов выше можно изменить. Так мы изменим объект **timedelta** прибавив часы к минутам:

```python
duration_hours = datetime.timedelta(hours = 1)
duration_minutes = datetime.timedelta(minutes = 10)
result = duration_hours + duration_minutes
print(result, type(result))
```

![](https://fixmypc.ru/media/uploads/2019/12/29/22.jpg)Изменение времени в Python datetime

С помощью **timedelta** изменяется и дата. Пример ниже изменяет текущую дату прибавляя к ней 1 день и 1 час:

```python
date_now = datetime.datetime.now()
duration_minutes = datetime.timedelta(days=1, minutes=10)
result = date_now + duration_minutes
print(result, type(result))
```

![](https://fixmypc.ru/media/uploads/2019/12/29/23.jpg)Добавление дней в Python datetime

### Сравнение и условия ###

Каких либо хитростей в сравнении объектов нет. В следующем примере идет сравнение двух дат с разницей в день:

```python
date_one = datetime.datetime(2017,12,1)
date_two = datetime.datetime(2017,12,2)
if date_two > date_one:
    print('{0} больше чем {1}'.format(date_two, date_one))
```

![](https://fixmypc.ru/media/uploads/2019/12/29/25.jpg)Сравнение объектов datetime в Python

При этом стоит проверять, что объекты относятся к классу **datetime**, а не являются строками. В следующем примере мы сравниваем года, но они уже относятся к численным:

```python
date_one = datetime.datetime(2017,12,1)
date_two = datetime.datetime(2017,12,2)
if date_two.year == date_one.year:
    print('Год одинаковый. Тип данных: ', type(date_two.year))
```

![](https://fixmypc.ru/media/uploads/2019/12/29/26.jpg)Сравнение годов в Python datetime

Объект **timedelta** тоже можно сравнивать:

```python
time_one = datetime.timedelta(seconds=2)
time_two = datetime.timedelta(minutes=1)
if time_one < time_two:
    print('Pass')
```

![](https://fixmypc.ru/media/uploads/2019/12/29/27.jpg)Сравнение timedelta в Python datetime

### Работа с метками (штампами) timestamp ###

При работе с API или в Unix системах мы можем увидеть отображение времени в таком формате:

```sh
1578238360
```

Данные числа обозначают количество секунд с 1 января 1970 года. Мы можем конвертировать данное число в понятный формат используя **datetime**:

```python
timestamp = date.fromtimestamp(1578238360)
print(timestamp)
```

![](https://fixmypc.ru/media/uploads/2020/01/05/34.jpg)Работа с UNIX timestamp Python datetime

Так же сработает если мы хотим получить и время:

```python
timestamp = datetime.fromtimestamp(1578238360)
print(timestamp)
```

![](https://fixmypc.ru/media/uploads/2020/01/05/35.jpg)Получение даты и времени из timestamp в Python datetime

Для конвертирования в **timestamp** используется метод с аналогичным названием:

```python
from datetime import datetime

date_now = datetime.now()
print(date_now.timestamp)
```

![](https://fixmypc.ru/media/uploads/2020/01/05/36.jpg)Создание timestamp в Python datetime

### Работа с часовыми поясами ###

К сожалению у меня нет большого опыта работы с часовыми поясами и примеры ниже не стоит рассматривать как лучшие практики. 

Библиотека **datetime** не хранит часовые пояса, данные о переводах часов (летнее и зимнее время) и високосных секундах. К тому же, некоторые страны, могут изменить время опираясь на локальные ситуации. Эти ситуации опасны, когда идет запись в базу данных. Для вывода в GUI, можно использовать **datetime.datetime.now()** или высчитывать часовой пояс из базы.

Для записи в базу данных мы можем использовать время UTC и отдельно считать часовой пояс:

```python
utc = datetime.datetime.utcnow()
print(utc)
```

![](https://fixmypc.ru/media/uploads/2019/12/30/30.jpg)Время UTC в Python Datetime

Следующий пример вычислит разницу времени между UTC и локальным. Насколько я знаю он может не сработать на версиях Python < 3.3:

```python
gmt = datetime.today() - datetime.utcnow()
print(gmt)
```

![](https://fixmypc.ru/media/uploads/2019/12/30/31.jpg)Разница во времени UTC Python datetime

Для вычисления других часовых поясов можно использовать стороннюю библиотеку **pytz**, которая их хранит:

```sh
pip install pytz
```

 Вывести все часовые пояса мы можем так:

```python
import pytz
for tz in pytz.all_timezones:
    print(tz)
```

![](https://fixmypc.ru/media/uploads/2020/01/02/32.jpg)Вывод всех часовых поясов в Python pytz