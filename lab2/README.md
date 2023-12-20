Лабораторная работа №2
================
Перов Кирилл

# Основы обработки данных с помощью R

## Цель

1.  Развить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания базовых типов данных языка R
3.  Развить пркатические навыки использования функций обработки данных
    пакета `dplyr` – функции select(), filter(), mutate(), arrange(),
    group_by()

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.0
4.  dplyr 1.1.4

## План

1.  Установить пакет `dplyr`
2.  Выполнить задания на наборе данных `starwars`

## Шаги

### Установка dplyr

Пакет `dplyr` можно установить в RStudio Desktop с помощью команды
`install.packages("dplyr")` Далее подключим пакет к текущему проекте с
помощью library(dplyr):

``` r
library(dplyr)
```


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

### Выполнение заданий на наборе данных ‘starwars’

#### 1. Сколько строк в датафрейме?

``` r
starwars %>%
  nrow()
```

    [1] 87

#### 2. Сколько столбцов в датафрейме?

``` r
starwars %>% 
  ncol()
```

    [1] 14

#### 3. Как просмотреть примерный вид датафрейма?

``` r
starwars %>%
  head()
```

    # A tibble: 6 × 14
      name      height  mass hair_color skin_color eye_color birth_year sex   gender
      <chr>      <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
    1 Luke Sky…    172    77 blond      fair       blue            19   male  mascu…
    2 C-3PO        167    75 <NA>       gold       yellow         112   none  mascu…
    3 R2-D2         96    32 <NA>       white, bl… red             33   none  mascu…
    4 Darth Va…    202   136 none       white      yellow          41.9 male  mascu…
    5 Leia Org…    150    49 brown      light      brown           19   fema… femin…
    6 Owen Lars    178   120 brown, gr… light      blue            52   male  mascu…
    # ℹ 5 more variables: homeworld <chr>, species <chr>, films <list>,
    #   vehicles <list>, starships <list>

#### 4. Сколько уникальных рас персонажей (species) представлено в данных?

``` r
starwars %>%
  distinct(species) %>%
  n_distinct()
```

    [1] 38

#### 5. Найти самого высокого персонажа.

``` r
starwars %>% 
  filter(height == max(height, na.rm = TRUE))
```

    # A tibble: 1 × 14
      name      height  mass hair_color skin_color eye_color birth_year sex   gender
      <chr>      <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
    1 Yarael P…    264    NA none       white      yellow            NA male  mascu…
    # ℹ 5 more variables: homeworld <chr>, species <chr>, films <list>,
    #   vehicles <list>, starships <list>

#### 6. Найти всех персонажей ниже 170

``` r
starwars %>%
  filter(height < 170)
```

    # A tibble: 23 × 14
       name     height  mass hair_color skin_color eye_color birth_year sex   gender
       <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
     1 C-3PO       167    75 <NA>       gold       yellow           112 none  mascu…
     2 R2-D2        96    32 <NA>       white, bl… red               33 none  mascu…
     3 Leia Or…    150    49 brown      light      brown             19 fema… femin…
     4 Beru Wh…    165    75 brown      light      blue              47 fema… femin…
     5 R5-D4        97    32 <NA>       white, red red               NA none  mascu…
     6 Yoda         66    17 white      green      brown            896 male  mascu…
     7 Mon Mot…    150    NA auburn     fair       blue              48 fema… femin…
     8 Wicket …     88    20 brown      brown      brown              8 male  mascu…
     9 Nien Nu…    160    68 none       grey       black             NA male  mascu…
    10 Watto       137    NA black      blue, grey yellow            NA male  mascu…
    # ℹ 13 more rows
    # ℹ 5 more variables: homeworld <chr>, species <chr>, films <list>,
    #   vehicles <list>, starships <list>

#### 7. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать по формуле 𝐼 = 𝑚/ℎ2 , где 𝑚 – масса (weight), а ℎ – рост (height).

``` r
starwars %>%
  mutate(BMI = mass / ((height / 100)^2)) %>%
  select (name, height, mass, BMI)
```

    # A tibble: 87 × 4
       name               height  mass   BMI
       <chr>               <int> <dbl> <dbl>
     1 Luke Skywalker        172    77  26.0
     2 C-3PO                 167    75  26.9
     3 R2-D2                  96    32  34.7
     4 Darth Vader           202   136  33.3
     5 Leia Organa           150    49  21.8
     6 Owen Lars             178   120  37.9
     7 Beru Whitesun lars    165    75  27.5
     8 R5-D4                  97    32  34.0
     9 Biggs Darklighter     183    84  25.1
    10 Obi-Wan Kenobi        182    77  23.2
    # ℹ 77 more rows

#### 8. Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по отношению массы (mass) к росту (height) персонажей.

``` r
starwars %>%
  mutate(long = mass / height) %>%
  arrange(desc(long)) %>%
  head(10)
```

    # A tibble: 10 × 15
       name     height  mass hair_color skin_color eye_color birth_year sex   gender
       <chr>     <int> <dbl> <chr>      <chr>      <chr>          <dbl> <chr> <chr> 
     1 Jabba D…    175  1358 <NA>       green-tan… orange         600   herm… mascu…
     2 Grievous    216   159 none       brown, wh… green, y…       NA   male  mascu…
     3 IG-88       200   140 none       metal      red             15   none  mascu…
     4 Owen La…    178   120 brown, gr… light      blue            52   male  mascu…
     5 Darth V…    202   136 none       white      yellow          41.9 male  mascu…
     6 Jek Ton…    180   110 brown      fair       blue            NA   male  mascu…
     7 Bossk       190   113 none       green      red             53   male  mascu…
     8 Tarfful     234   136 brown      brown      blue            NA   male  mascu…
     9 Dexter …    198   102 none       brown      yellow          NA   male  mascu…
    10 Chewbac…    228   112 brown      unknown    blue           200   male  mascu…
    # ℹ 6 more variables: homeworld <chr>, species <chr>, films <list>,
    #   vehicles <list>, starships <list>, long <dbl>

#### 9. Найти средний возраст персонажей каждой расы вселенной Звездных войн.

``` r
starwars %>%
  group_by(species) %>%
  summarize(mean_age = mean(birth_year, na.rm = TRUE))
```

    # A tibble: 38 × 2
       species   mean_age
       <chr>        <dbl>
     1 Aleena       NaN  
     2 Besalisk     NaN  
     3 Cerean        92  
     4 Chagrian     NaN  
     5 Clawdite     NaN  
     6 Droid         53.3
     7 Dug          NaN  
     8 Ewok           8  
     9 Geonosian    NaN  
    10 Gungan        52  
    # ℹ 28 more rows

#### 10. Найти самый распространенный цвет глаз персонажей вселенной Звездных войн.

``` r
starwars %>%
  group_by(eye_color) %>%
  summarize(count = n()) %>%
  arrange(desc(count)) %>%
  head(1)
```

    # A tibble: 1 × 2
      eye_color count
      <chr>     <int>
    1 brown        21

#### 11. Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн.

``` r
starwars %>%
  group_by(species) %>%
  summarize(mean_name_length = mean(nchar(name), na.rm = TRUE))
```

    # A tibble: 38 × 2
       species   mean_name_length
       <chr>                <dbl>
     1 Aleena               13   
     2 Besalisk             15   
     3 Cerean               12   
     4 Chagrian             10   
     5 Clawdite             10   
     6 Droid                 4.83
     7 Dug                   7   
     8 Ewok                 21   
     9 Geonosian            17   
    10 Gungan               11.7 
    # ℹ 28 more rows

## Оценка результата

В результате данной лабораторной работы были выполнены задания с
использованием пакета dplyr

## Вывод

Мы получили базовые навыки работы с пакетом dplyr для языка R
