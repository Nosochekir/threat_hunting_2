Лабораторная работа №4
================
Перов Кирилл

# Исследование метаданных DNS трафика

## Цель

1.  Зекрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    `tidyverse` языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.2
4.  dplyr 1.1.4
5.  tidyverse 2.0.0
6.  dns.log
7.  header.csv

## План

1.  Установить пакет ‘dplyr’
2.  Импорт и подготовка данных DNS
3.  Анализ данных
4.  Обогащение данных

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

### Установка tidyverse

Устанавливаем пакет `tidyverse` с помощью команды
`install.packages("tidyverse")`. Затем подключаем к текущему проекту:

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ forcats   1.0.0     ✔ readr     2.1.4
    ✔ ggplot2   3.4.4     ✔ stringr   1.5.1
    ✔ lubridate 1.9.3     ✔ tibble    3.2.1
    ✔ purrr     1.0.2     ✔ tidyr     1.3.0
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

### Импорт и подготовка данных DNS

#### 1. Импортируйте данные DNS

``` r
dns <- read.table("dns.log", header = FALSE, sep = "\t", quote = "", encoding="UTF-8")
```

#### 2. Добавьте пропущенные данные о структуре данных (назначении столбцов)

В файле header.csv не хватает некоторых данных о столбцах, поэтому,
используя открытые источники, мы впишем их вручную.

``` r
colnames(dns) <- read.csv("header.csv", header = FALSE, skip = 1)$V1
```

#### 3. Просмотрите общую структуру данных с помощью функции glimpse()

``` r
dns %>%
  glimpse()
```

    Rows: 427,935
    Columns: 23
    $ ts          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901006…
    $ uid         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz7Bs…
    $ id.orig_h   <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", "19…
    $ id.orig_p   <int> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 1…
    $ id.resp_h   <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255", "1…
    $ id_resp_p   <int> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ proto       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp", "u…
    $ trans_id    <int> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187, 62…
    $ query       <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\…
    $ qclass      <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1"…
    $ qclass_name <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET", "C…
    $ qtype       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "32"…
    $ qtype_name  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB…
    $ rcode       <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rcode_name  <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-…
    $ AA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ TC          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ RD          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRU…
    $ RA          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…
    $ Z           <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0…
    $ answers     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ TTLs        <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-"…
    $ rejected    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FA…

### Анализ данных

#### 4. Сколько участников информационного обмена в сети Доброй Организации?

``` r
hosts <- union(unique(dns$id.orig_h), unique(dns$id.resp_h))
hosts %>% length()
```

    [1] 1359

#### 5. Какое соотношение участников обмена внутри сети и участников обращений к внешним ресурсам?

``` r
internal_ip_pattern <- c("192.168.", "10.", "100.([6-9]|1[0-1][0-9]|12[0-7]).", "172.((1[6-9])|(2[0-9])|(3[0-1])).")
internal_ips <- hosts[grep(paste(internal_ip_pattern, collapse = "|"), hosts)]
internal_ips_cnt <- sum(hosts %in% internal_ips)
external_ips_cnt <- length(hosts) - internal_ips_cnt

rat <- internal_ips_cnt / external_ips_cnt
rat
```

    [1] 15.57317

#### 6. Найдите топ-10 участников сети, проявляющих наибольшую сетевую активность.

``` r
top_host <- dns %>%
  group_by(id.orig_h) %>%
  summarise(request_count = n()) %>%
  arrange(desc(request_count)) %>%
  top_n(10, request_count)
top_host
```

    # A tibble: 10 × 2
       id.orig_h       request_count
       <chr>                   <int>
     1 10.10.117.210           75943
     2 192.168.202.93          26522
     3 192.168.202.103         18121
     4 192.168.202.76          16978
     5 192.168.202.97          16176
     6 192.168.202.141         14967
     7 10.10.117.209           14222
     8 192.168.202.110         13372
     9 192.168.203.63          12148
    10 192.168.202.106         10784

#### 7. Найдите топ-10 доменов, к которым обращаются пользователи сети и соответственное количество обращений

``` r
top_domain <- dns %>%
  group_by(query) %>%
  summarise(request_count = n()) %>%
  arrange(desc(request_count)) %>%
  top_n(10, request_count)
top_domain
```

    # A tibble: 10 × 2
       query                                                           request_count
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00…         10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7248
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569

#### 8. Опеределите базовые статистические характеристики (функция summary()) интервала времени между последовательным обращениями к топ-10 доменам.

``` r
top_domain_filter <- dns %>% 
  filter(tolower(query) %in% top_domain$query) %>%
  arrange(ts)
time_interval <- diff(top_domain_filter$ts)
summary(time_interval)
```

        Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
        0.00     0.00     0.10     1.07     0.54 49677.59 

#### 9. Часто вредоносное программное обеспечение использует DNS канал в качестве канала управления, периодически отправляя запросы на подконтрольный злоумышленникам DNS сервер. По периодическим запросам на один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP адреса в исследуемом датасете?

``` r
ip_domain_counts <- dns %>%
  group_by(ip = `id.orig_h`, query) %>%
  summarise(request_count = n()) %>%
  filter(request_count > 1)
```

    `summarise()` has grouped output by 'ip'. You can override using the `.groups`
    argument.

``` r
unique_ips_with_periodic_requests <- unique(ip_domain_counts$ip)

unique_ips_with_periodic_requests %>% length()
```

    [1] 240

``` r
unique_ips_with_periodic_requests %>% head()
```

    [1] "10.10.10.10"     "10.10.117.209"   "10.10.117.210"   "128.244.37.196" 
    [5] "169.254.109.123" "169.254.228.26" 

### Обогащение данных

#### 10. Определите местоположение (страну, город) и организацию-провайдера для топ-10 доменов. Для этого можно использовать сторонние сервисы, например https://v4.ifconfig.co/.

``` r
top_domain
```

    # A tibble: 10 × 2
       query                                                           request_count
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00…         10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7248
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569

1.  teredo.ipv6.microsoft.com

    -   IP: 20.112.250.133
    -   Country: United States
    -   Timezone: America/Chicago
    -   Organization: Microsoft

2.  tools.google.com

    -   IP: 173.194.222.100
    -   Country: United States
    -   Timezone: America/Chicago
    -   Organization: Google

3.  www.apple.com

    -   IP: 17.253.144.10
    -   Country: United States
    -   Timezone: America/Chicago
    -   Organization: Apple-Engineering

4.  safebrowsing.clients.google.com

    -   IP: 64.233.164.100
    -   Country: United States
    -   Timezone: America/Chicago
    -   Organization: Google

5.  44.206.168.192.in-addr.arpa

    -   IP: 44.206.168.192
    -   Country: United States
    -   City: Ashburn
    -   Timezone: America/New_York
    -   Organization: Amazon

## Оценка результата

В результате лабораторной работы были выполнены задания с использованием
пакета `dplyr` и `tidyverse`

## Вывод

В ходе лабораторной работы были проанализированы и дополнены данные
трафика DNS.
