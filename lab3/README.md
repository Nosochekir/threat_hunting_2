Лабораторная работа №3
================
Перов Кирилл

# Основы обработки данных с помощью R

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить пркатические навыки использования функций обработки данных
    пакета `dplyr` – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.2
4.  dplyr 1.2.2
5.  nycflights13 1.0.2

## План

1.  Установить пакет `dplyr`
2.  Установить пакет `nycflights13`
3.  Выполнить задания

## Шаги

### Установка dplyr

Устанавливаем пакет `dplyr` с помощью команды
`install.packages("dplyr")`. Затем подключаем к текущему проекту:

``` r
library(dplyr)
```


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

### Установка nycflights13

Устанавливаем пакет `nycflights13` с помощью команды
`install.packages("nycflights13")`. Затем подключаем к текущему проекту:

``` r
library(nycflights13)
```

### Выполнение заданий

#### 1. Сколько встроенных в пакет nycflights13 датафреймов?

В пакет `nycflights13` встроено 5 датафреймов: airlines airports flights
planes weather

#### 2. Сколько строк в каждом датафрейме?

``` r
airlines %>% 
  nrow()
```

    [1] 16

``` r
airports %>%
  nrow()
```

    [1] 1458

``` r
flights %>%
  nrow()
```

    [1] 336776

``` r
planes %>%
  nrow()
```

    [1] 3322

``` r
weather %>%
  nrow()
```

    [1] 26115

#### 3. Сколько столбцов в каждом датафрейме?

``` r
airlines %>% 
  ncol()
```

    [1] 2

``` r
airports %>%
  ncol()
```

    [1] 8

``` r
flights %>%
  ncol()
```

    [1] 19

``` r
planes %>%
  ncol()
```

    [1] 9

``` r
weather %>%
  ncol()
```

    [1] 15

#### 4. Как просмотреть примерный вид датафрейма?

``` r
weather %>%
  head()
```

    # A tibble: 6 × 15
      origin  year month   day  hour  temp  dewp humid wind_dir wind_speed wind_gust
      <chr>  <int> <int> <int> <int> <dbl> <dbl> <dbl>    <dbl>      <dbl>     <dbl>
    1 EWR     2013     1     1     1  39.0  26.1  59.4      270      10.4         NA
    2 EWR     2013     1     1     2  39.0  27.0  61.6      250       8.06        NA
    3 EWR     2013     1     1     3  39.0  28.0  64.4      240      11.5         NA
    4 EWR     2013     1     1     4  39.9  28.0  62.2      250      12.7         NA
    5 EWR     2013     1     1     5  39.0  28.0  64.4      260      12.7         NA
    6 EWR     2013     1     1     6  37.9  28.0  67.2      240      11.5         NA
    # ℹ 4 more variables: precip <dbl>, pressure <dbl>, visib <dbl>,
    #   time_hour <dttm>

#### 5. Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?

``` r
table(flights$carrier) %>%
  nrow()
```

    [1] 16

#### 6. Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

``` r
flights %>% 
  filter( month==5, origin=='JFK') %>%
  nrow()
```

    [1] 9397

#### 7. Какой самый северный аэропорт?

``` r
airports %>%
  filter(lat == max(lat))
```

    # A tibble: 1 × 8
      faa   name                      lat   lon   alt    tz dst   tzone
      <chr> <chr>                   <dbl> <dbl> <dbl> <dbl> <chr> <chr>
    1 EEN   Dillant Hopkins Airport  72.3  42.9   149    -5 A     <NA> 

#### 8. Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?

``` r
airports %>%
  filter(alt == max(alt))
```

    # A tibble: 1 × 8
      faa   name        lat   lon   alt    tz dst   tzone         
      <chr> <chr>     <dbl> <dbl> <dbl> <dbl> <chr> <chr>         
    1 TEX   Telluride  38.0 -108.  9078    -7 A     America/Denver

#### 9. Какие бортовые номера у самых старых самолетов?

``` r
planes %>%
  arrange(year) %>%
  head(10) %>%
  select(tailnum)
```

    # A tibble: 10 × 1
       tailnum
       <chr>  
     1 N381AA 
     2 N201AA 
     3 N567AA 
     4 N378AA 
     5 N575AA 
     6 N14629 
     7 N615AA 
     8 N425AA 
     9 N383AA 
    10 N364AA 

#### 10. Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия).

``` r
weather %>% 
  filter(origin == 'JFK' & month == 9) %>%
  summarise(mean_temp = mean(temp, na.rm = TRUE))
```

    # A tibble: 1 × 1
      mean_temp
          <dbl>
    1      66.9

#### 11. Самолеты какой авиакомпании совершили больше всего вылетов в июне?

``` r
flights %>%
  filter(month == 6) %>%
  group_by(carrier) %>%
  summarize(total_flights = n()) %>%
  arrange(desc(total_flights)) %>%
  head(1)
```

    # A tibble: 1 × 2
      carrier total_flights
      <chr>           <int>
    1 UA               4975

#### 12. Самолеты какой авиакомпании задерживались чаще других в 2013 году?

``` r
flights %>%
  filter(year == 2013) %>%
  group_by(carrier) %>%
  summarize(count_delay = sum(arr_delay > 0, na.rm = TRUE)) %>%
  arrange(desc(count_delay)) %>%
  head(1)
```

    # A tibble: 1 × 2
      carrier count_delay
      <chr>         <int>
    1 EV            24484

## Оценка результата

В результате лабораторной работы были выполнены задания из датафреймов
пакета nycflights13 с использованием пакета dplyr.

## Вывод

Мы получили базовые навыки работы с пакетом dplyr для языка R с новыми
наборами данных
