Лабораторная работа №5
================
Перов Кирилл

# Исследование информации о состоянии беспроводных сетей

## Цель

1.  Получить знания о методах исследования радиоэлектронной обстановки
2.  Составить представление о механизмах работы Wi-Fi сетей на канальном
    и сетевом уровне модели OSI
3.  Закрепить практические навыки использования языка программирования R
    для обработки данных
4.  Закрепить знания основных функций обработки данных экосистемы
    `tidyverse` языка R

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.3.2
4.  dplyr 1.1.4

## Шаги

### Установка пакетов

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

Устанавливаем пакет `lubridate` с помощью команды
`install.packages("lubridate")`. Затем подключаем к текущему проекту:

``` r
library(lubridate)
```


    Присоединяю пакет: 'lubridate'

    Следующие объекты скрыты от 'package:base':

        date, intersect, setdiff, union

### Импорт и подготовка данных

#### Импортируйте данные.

``` r
data <- read.csv("mir.csv-01.csv")
head(data)
```

                  BSSID      First.time.seen       Last.time.seen channel Speed
    1 BE:F1:71:D5:17:8B  2023-07-28 09:13:03  2023-07-28 11:50:50       1   195
    2 6E:C7:EC:16:DA:1A  2023-07-28 09:13:03  2023-07-28 11:55:12       1   130
    3 9A:75:A8:B9:04:1E  2023-07-28 09:13:03  2023-07-28 11:53:31       1   360
    4 4A:EC:1E:DB:BF:95  2023-07-28 09:13:03  2023-07-28 11:04:01       7   360
    5 D2:6D:52:61:51:5D  2023-07-28 09:13:03  2023-07-28 10:30:19       6   130
    6 E8:28:C1:DC:B2:52  2023-07-28 09:13:03  2023-07-28 11:55:38       6   130
      Privacy Cipher Authentication Power X..beacons     X..IV           LAN.IP
    1    WPA2   CCMP            PSK   -30        846       504    0.  0.  0.  0
    2    WPA2   CCMP            PSK   -30        750       116    0.  0.  0.  0
    3    WPA2   CCMP            PSK   -68        694        26    0.  0.  0.  0
    4    WPA2   CCMP            PSK   -37        510        21    0.  0.  0.  0
    5    WPA2   CCMP            PSK   -57        647         6    0.  0.  0.  0
    6     OPN                         -63        251      3430  172. 17.203.197
      ID.length           ESSID Key
    1        12    C322U13 3965  NA
    2         4            Cnet  NA
    3         2              KC  NA
    4        14  POCO X5 Pro 5G  NA
    5        25                  NA
    6        13   MIREA_HOTSPOT  NA

#### Привести датасеты в вид “аккуратных данных”, преобразовать типы столбцов в соответствии с типом данных.

data_1 - анонсы беспроводных точек доступа

data_2 - запросы на подключение клиентов к известным им точкам доступа

``` r
data_1 = read.csv("mir.csv-01.csv", nrows = 167)
data_1 %>% head()
```

                  BSSID      First.time.seen       Last.time.seen channel Speed
    1 BE:F1:71:D5:17:8B  2023-07-28 09:13:03  2023-07-28 11:50:50       1   195
    2 6E:C7:EC:16:DA:1A  2023-07-28 09:13:03  2023-07-28 11:55:12       1   130
    3 9A:75:A8:B9:04:1E  2023-07-28 09:13:03  2023-07-28 11:53:31       1   360
    4 4A:EC:1E:DB:BF:95  2023-07-28 09:13:03  2023-07-28 11:04:01       7   360
    5 D2:6D:52:61:51:5D  2023-07-28 09:13:03  2023-07-28 10:30:19       6   130
    6 E8:28:C1:DC:B2:52  2023-07-28 09:13:03  2023-07-28 11:55:38       6   130
      Privacy Cipher Authentication Power X..beacons X..IV           LAN.IP
    1    WPA2   CCMP            PSK   -30        846   504    0.  0.  0.  0
    2    WPA2   CCMP            PSK   -30        750   116    0.  0.  0.  0
    3    WPA2   CCMP            PSK   -68        694    26    0.  0.  0.  0
    4    WPA2   CCMP            PSK   -37        510    21    0.  0.  0.  0
    5    WPA2   CCMP            PSK   -57        647     6    0.  0.  0.  0
    6     OPN                         -63        251  3430  172. 17.203.197
      ID.length           ESSID Key
    1        12    C322U13 3965  NA
    2         4            Cnet  NA
    3         2              KC  NA
    4        14  POCO X5 Pro 5G  NA
    5        25                  NA
    6        13   MIREA_HOTSPOT  NA

``` r
data_2 = read.csv("mir.csv-01.csv", skip = 170)
data_2 %>% head()
```

            Station.MAC      First.time.seen       Last.time.seen Power X..packets
    1 CA:66:3B:8F:56:DD  2023-07-28 09:13:03  2023-07-28 10:59:44   -33        858
    2 96:35:2D:3D:85:E6  2023-07-28 09:13:03  2023-07-28 09:13:03   -65          4
    3 5C:3A:45:9E:1A:7B  2023-07-28 09:13:03  2023-07-28 11:51:54   -39        432
    4 C0:E4:34:D8:E7:E5  2023-07-28 09:13:03  2023-07-28 11:53:16   -61        958
    5 5E:8E:A6:5E:34:81  2023-07-28 09:13:04  2023-07-28 09:13:04   -53          1
    6 10:51:07:CB:33:E7  2023-07-28 09:13:05  2023-07-28 11:56:06   -43        344
                   BSSID Probed.ESSIDs
    1  BE:F1:71:D5:17:8B  C322U13 3965
    2  (not associated)   IT2 Wireless
    3  BE:F1:71:D6:10:D7  C322U21 0566
    4  BE:F1:71:D5:17:8B  C322U13 3965
    5  (not associated)               
    6  (not associated)               

``` r
data_1 <- data_1 %>% 
  mutate_at(vars(BSSID, Privacy, Cipher, Authentication, LAN.IP, ESSID), trimws) %>%
  mutate_at(vars(BSSID, Privacy, Cipher, Authentication, LAN.IP, ESSID), na_if, "")


data_1$First.time.seen <- as.POSIXct(data_1$First.time.seen, origin="1970-01-01")
data_1$Last.time.seen <- as.POSIXct(data_1$Last.time.seen, origin="1970-01-01")

data_1 %>% head
```

                  BSSID     First.time.seen      Last.time.seen channel Speed
    1 BE:F1:71:D5:17:8B 2023-07-28 09:13:03 2023-07-28 11:50:50       1   195
    2 6E:C7:EC:16:DA:1A 2023-07-28 09:13:03 2023-07-28 11:55:12       1   130
    3 9A:75:A8:B9:04:1E 2023-07-28 09:13:03 2023-07-28 11:53:31       1   360
    4 4A:EC:1E:DB:BF:95 2023-07-28 09:13:03 2023-07-28 11:04:01       7   360
    5 D2:6D:52:61:51:5D 2023-07-28 09:13:03 2023-07-28 10:30:19       6   130
    6 E8:28:C1:DC:B2:52 2023-07-28 09:13:03 2023-07-28 11:55:38       6   130
      Privacy Cipher Authentication Power X..beacons X..IV          LAN.IP
    1    WPA2   CCMP            PSK   -30        846   504   0.  0.  0.  0
    2    WPA2   CCMP            PSK   -30        750   116   0.  0.  0.  0
    3    WPA2   CCMP            PSK   -68        694    26   0.  0.  0.  0
    4    WPA2   CCMP            PSK   -37        510    21   0.  0.  0.  0
    5    WPA2   CCMP            PSK   -57        647     6   0.  0.  0.  0
    6     OPN   <NA>           <NA>   -63        251  3430 172. 17.203.197
      ID.length          ESSID Key
    1        12   C322U13 3965  NA
    2         4           Cnet  NA
    3         2             KC  NA
    4        14 POCO X5 Pro 5G  NA
    5        25           <NA>  NA
    6        13  MIREA_HOTSPOT  NA

``` r
data_2 <- data_2 %>% 
  mutate_at(vars(Station.MAC, BSSID, Probed.ESSIDs), trimws) %>%
  mutate_at(vars(Station.MAC, BSSID, Probed.ESSIDs), na_if, "")

data_2$First.time.seen <- as.POSIXct(data_2$First.time.seen, format = "%Y-%m-%d %H:%M:%S")
data_2$Last.time.seen <- as.POSIXct(data_2$Last.time.seen, format = "%Y-%m-%d %H:%M:%S")

data_2 %>% head
```

            Station.MAC     First.time.seen      Last.time.seen Power X..packets
    1 CA:66:3B:8F:56:DD 2023-07-28 09:13:03 2023-07-28 10:59:44   -33        858
    2 96:35:2D:3D:85:E6 2023-07-28 09:13:03 2023-07-28 09:13:03   -65          4
    3 5C:3A:45:9E:1A:7B 2023-07-28 09:13:03 2023-07-28 11:51:54   -39        432
    4 C0:E4:34:D8:E7:E5 2023-07-28 09:13:03 2023-07-28 11:53:16   -61        958
    5 5E:8E:A6:5E:34:81 2023-07-28 09:13:04 2023-07-28 09:13:04   -53          1
    6 10:51:07:CB:33:E7 2023-07-28 09:13:05 2023-07-28 11:56:06   -43        344
                  BSSID Probed.ESSIDs
    1 BE:F1:71:D5:17:8B  C322U13 3965
    2  (not associated)  IT2 Wireless
    3 BE:F1:71:D6:10:D7  C322U21 0566
    4 BE:F1:71:D5:17:8B  C322U13 3965
    5  (not associated)          <NA>
    6  (not associated)          <NA>

### Анализ данных

#### Точки доступа

##### 1. Определить небезопасные точки доступа (без шифрования – OPN)

``` r
opn_wifi <- data_1 %>% 
  filter(grepl("OPN", Privacy)) %>%
  select(BSSID, ESSID) %>%
  arrange(BSSID) %>%
  distinct

opn_wifi
```

                   BSSID         ESSID
    1  00:03:7A:1A:03:56       MT_FREE
    2  00:03:7F:12:34:56       MT_FREE
    3  00:25:00:FF:94:73          <NA>
    4  00:26:99:F2:7A:E0          <NA>
    5  00:26:99:F2:7A:EF          <NA>
    6  00:3E:1A:5D:14:45       MT_FREE
    7  00:53:7A:99:98:56       MT_FREE
    8  00:AB:0A:00:10:10          <NA>
    9  02:67:F1:B0:6C:98       MT_FREE
    10 02:BC:15:7E:D5:DC       MT_FREE
    11 02:CF:8B:87:B4:F9       MT_FREE
    12 E0:D9:E3:48:FF:D2          <NA>
    13 E0:D9:E3:49:00:B1          <NA>
    14 E8:28:C1:DC:0B:B2          <NA>
    15 E8:28:C1:DC:33:12          <NA>
    16 E8:28:C1:DC:B2:40 MIREA_HOTSPOT
    17 E8:28:C1:DC:B2:41  MIREA_GUESTS
    18 E8:28:C1:DC:B2:42          <NA>
    19 E8:28:C1:DC:B2:50  MIREA_GUESTS
    20 E8:28:C1:DC:B2:51          <NA>
    21 E8:28:C1:DC:B2:52 MIREA_HOTSPOT
    22 E8:28:C1:DC:BD:50  MIREA_GUESTS
    23 E8:28:C1:DC:BD:52 MIREA_HOTSPOT
    24 E8:28:C1:DC:C6:B0  MIREA_GUESTS
    25 E8:28:C1:DC:C6:B1          <NA>
    26 E8:28:C1:DC:C6:B2          <NA>
    27 E8:28:C1:DC:C8:30  MIREA_GUESTS
    28 E8:28:C1:DC:C8:31          <NA>
    29 E8:28:C1:DC:C8:32 MIREA_HOTSPOT
    30 E8:28:C1:DC:FF:F2          <NA>
    31 E8:28:C1:DD:04:40 MIREA_HOTSPOT
    32 E8:28:C1:DD:04:41  MIREA_GUESTS
    33 E8:28:C1:DD:04:42          <NA>
    34 E8:28:C1:DD:04:50  MIREA_GUESTS
    35 E8:28:C1:DD:04:51          <NA>
    36 E8:28:C1:DD:04:52 MIREA_HOTSPOT
    37 E8:28:C1:DE:47:D0  MIREA_GUESTS
    38 E8:28:C1:DE:47:D1          <NA>
    39 E8:28:C1:DE:47:D2 MIREA_HOTSPOT
    40 E8:28:C1:DE:74:30          <NA>
    41 E8:28:C1:DE:74:31          <NA>
    42 E8:28:C1:DE:74:32 MIREA_HOTSPOT

##### 2. Определить производителя для каждого обнаруженного устройства

E0:D9:E3 Eltex Enterprise Ltd. 00:26:99 Cisco Systems, Inc 00:25:00
Apple, Inc. 00:03:7F Atheros Communications, Inc.

##### 3. Выявить устройства, использующие последнюю версию протокола шифрования WPA3, и названия точек доступа, реализованных на этих устройствах

``` r
data_1 %>%
  filter(grepl("WPA3", Privacy)) %>%
  select(BSSID, ESSID, Privacy)
```

                  BSSID              ESSID   Privacy
    1 26:20:53:0C:98:E8               <NA> WPA3 WPA2
    2 A2:FE:FF:B8:9B:C9         Christie’s WPA3 WPA2
    3 96:FF:FC:91:EF:64               <NA> WPA3 WPA2
    4 CE:48:E7:86:4E:33 iPhone (Анастасия) WPA3 WPA2
    5 8E:1F:94:96:DA:FD iPhone (Анастасия) WPA3 WPA2
    6 BE:FD:EF:18:92:44            Димасик WPA3 WPA2
    7 3A:DA:00:F9:0C:02  iPhone XS Max 🦊🐱🦊 WPA3 WPA2
    8 76:C5:A0:70:08:96               <NA> WPA3 WPA2

##### 4. Отсортировать точки доступа по интервалу времени, в течение которого они находились на связи, по убыванию.

``` r
data_1_intervals <- data_1 %>% 
  mutate(time_inter = Last.time.seen - First.time.seen)

data_1_intervals %>%
  arrange(desc(time_inter)) %>%
  mutate(time_inter = seconds_to_period(time_inter)) %>%
  select(BSSID, First.time.seen, Last.time.seen, time_inter) %>%
  head
```

                  BSSID     First.time.seen      Last.time.seen time_inter
    1 00:25:00:FF:94:73 2023-07-28 09:13:06 2023-07-28 11:56:21 2H 43M 15S
    2 E8:28:C1:DD:04:52 2023-07-28 09:13:09 2023-07-28 11:56:05 2H 42M 56S
    3 E8:28:C1:DC:B2:52 2023-07-28 09:13:03 2023-07-28 11:55:38 2H 42M 35S
    4 08:3A:2F:56:35:FE 2023-07-28 09:13:27 2023-07-28 11:55:53 2H 42M 26S
    5 6E:C7:EC:16:DA:1A 2023-07-28 09:13:03 2023-07-28 11:55:12  2H 42M 9S
    6 E8:28:C1:DC:B2:50 2023-07-28 09:13:06 2023-07-28 11:55:12  2H 42M 6S

##### 5. Обнаружить топ-10 самых быстрых точек доступа

``` r
ten_fast <- data_1 %>%
  arrange(desc(Speed)) %>%
  select(BSSID, ESSID, Speed, Privacy) %>%
  head(10)

ten_fast
```

                   BSSID              ESSID Speed   Privacy
    1  26:20:53:0C:98:E8               <NA>   866 WPA3 WPA2
    2  96:FF:FC:91:EF:64               <NA>   866 WPA3 WPA2
    3  CE:48:E7:86:4E:33 iPhone (Анастасия)   866 WPA3 WPA2
    4  8E:1F:94:96:DA:FD iPhone (Анастасия)   866 WPA3 WPA2
    5  9A:75:A8:B9:04:1E                 KC   360      WPA2
    6  4A:EC:1E:DB:BF:95     POCO X5 Pro 5G   360      WPA2
    7  56:C5:2B:9F:84:90         OnePlus 6T   360      WPA2
    8  E8:28:C1:DC:B2:41       MIREA_GUESTS   360       OPN
    9  E8:28:C1:DC:B2:40      MIREA_HOTSPOT   360       OPN
    10 E8:28:C1:DC:B2:42               <NA>   360       OPN

##### 6. Отсортировать точки доступа по частоте отправки запросов (beacons) в единицу времени по их убыванию.

``` r
data_1_beacons <- data_1_intervals %>% 
    mutate(beacon_rate = as.double(X..beacons) / as.integer(time_inter))
```

``` r
data_1_beacons %>%
  arrange(desc(beacon_rate)) %>%
  select(BSSID, Privacy, ESSID, beacon_rate) %>%
  filter(beacon_rate != 0) 
```

                    BSSID   Privacy                       ESSID  beacon_rate
    1   76:E4:ED:B0:5C:9A      WPA2              Инет от Путина          Inf
    2   C2:B5:D7:7F:07:A8      WPA2 DIRECT-a8-HP M227f LaserJet          Inf
    3   E8:28:C1:DE:47:D1       OPN                        <NA>          Inf
    4   A2:FE:FF:B8:9B:C9 WPA3 WPA2                  Christie’s          Inf
    5   BA:2A:7A:DD:38:3E      WPA2                Айфон (Oleg)          Inf
    6   76:5E:F3:F9:A5:1C      WPA2                Redmi 9C NFC          Inf
    7   00:03:7F:12:34:56       OPN                     MT_FREE          Inf
    8   E0:D9:E3:49:00:B1       OPN                        <NA>          Inf
    9   E8:28:C1:DC:BD:52       OPN               MIREA_HOTSPOT          Inf
    10  00:26:CB:AA:62:72      WPA2                        GIVC          Inf
    11  6A:B0:1A:C2:DF:49      WPA2                        <NA>          Inf
    12  02:CF:8B:87:B4:F9       OPN                     MT_FREE          Inf
    13  E8:28:C1:DE:47:D0       OPN                MIREA_GUESTS          Inf
    14  F2:30:AB:E9:03:ED      WPA2             iPhone (Uliana) 0.8571428571
    15  B2:CF:C0:00:4A:60      WPA2         Михаил's Galaxy M32 0.8000000000
    16  3A:DA:00:F9:0C:02 WPA3 WPA2           iPhone XS Max 🦊🐱🦊 0.5555555556
    17  02:BC:15:7E:D5:DC       OPN                     MT_FREE 0.5000000000
    18  00:3E:1A:5D:14:45       OPN                     MT_FREE 0.5000000000
    19  76:C5:A0:70:08:96 WPA3 WPA2                        <NA> 0.5000000000
    20  D2:25:91:F6:6C:D8      WPA2                        Саня 0.3846153846
    21  BE:F1:71:D6:10:D7      WPA2                C322U21 0566 0.1740830779
    22  00:03:7A:1A:03:56       OPN                     MT_FREE 0.1666666667
    23  38:1A:52:0D:84:D7      WPA2    EBFCD57F-EE81fI_DL_1AO2T 0.1630006946
    24  0A:C5:E1:DB:17:7B      WPA2               AndroidAP177B 0.1453299257
    25  1E:93:E3:1B:3C:F4      WPA2                  Galaxy A71 0.1442956504
    26  D2:6D:52:61:51:5D      WPA2                        <NA> 0.1395599655
    27  BE:F1:71:D5:0E:53      WPA2                C322U06 9080 0.1347750109
    28  4A:86:77:04:B7:28      WPA2           iPhone (Искандер) 0.1200132979
    29  3A:70:96:C6:30:2C      WPA2           iPhone (Искандер) 0.1115384615
    30  76:70:AF:A4:D2:AF      WPA2                        витя 0.0925722649
    31  BE:F1:71:D5:17:8B      WPA2                C322U13 3965 0.0893630506
    32  AA:F4:3F:EE:49:0B      WPA2            Redmi Note 8 Pro 0.0815920398
    33  6E:C7:EC:16:DA:1A      WPA2                        Cnet 0.0770891150
    34  4A:EC:1E:DB:BF:95      WPA2              POCO X5 Pro 5G 0.0765995795
    35  56:C5:2B:9F:84:90      WPA2                  OnePlus 6T 0.0759645339
    36  9A:75:A8:B9:04:1E      WPA2                          KC 0.0720814292
    37  9C:A5:13:28:D5:89      WPA2              Galaxy M012063 0.0697674419
    38  36:46:53:81:12:A0      WPA2          GGWPRedmi Note 10S 0.0657051282
    39  38:1A:52:0D:85:1D      WPA2    EBFCD5C1-EE81fI_DN11AOcY 0.0624399616
    40  38:1A:52:0D:8F:EC      WPA2    EBFCD6C2-EE81fI_DR21AOa0 0.0406072106
    41  2E:FE:13:D0:96:51      WPA2            Redmi Note 8 Pro 0.0344827586
    42  CE:48:E7:86:4E:33 WPA3 WPA2          iPhone (Анастасия) 0.0305084746
    43  8E:1F:94:96:DA:FD WPA3 WPA2          iPhone (Анастасия) 0.0289156627
    44  E8:28:C1:DC:B2:51       OPN                        <NA> 0.0286889460
    45  E8:28:C1:DC:B2:50       OPN                MIREA_GUESTS 0.0267324697
    46  5E:C7:C0:E4:D7:D4      WPA2                   realme 10 0.0260336907
    47  E8:28:C1:DC:B2:52       OPN               MIREA_HOTSPOT 0.0257303947
    48  8E:55:4A:85:5B:01      WPA2                    Vladimir 0.0255065309
    49  38:1A:52:0D:90:5D      WPA2    EBFCD615-EE81fI_DOL1AO8w 0.0211515864
    50  1C:7E:E5:8E:B7:DE      WPA2                        <NA> 0.0149097018
    51  38:1A:52:0D:90:A1      WPA2    EBFCD597-EE81fI_DMN1AOe1 0.0129315322
    52  A2:64:E8:97:58:EE      WPA2                    Mi Phone 0.0122295390
    53  1E:C2:8E:D8:30:91      WPA2                        Damy 0.0120481928
    54  48:5B:39:F9:7A:48      WPA2                        <NA> 0.0112082262
    55  00:26:99:F2:7A:E2      WPA2                        GIVC 0.0086535490
    56  38:1A:52:0D:97:60      WPA2    EBFCD593-EE81fI_DMJ1AOI4 0.0068526676
    57  00:26:99:F2:7A:E1      WPA2                         IKB 0.0068478719
    58  00:26:99:BA:75:80      WPA2                        GIVC 0.0062821833
    59  A6:02:B9:73:2F:76      WPA2                C239U51 0701 0.0062741313
    60  9E:A3:A9:D6:28:3C      WPA2                        <NA> 0.0053962544
    61  00:23:EB:E3:81:FE      WPA2                         IKB 0.0050510478
    62  00:23:EB:E3:81:FD      WPA2                        GIVC 0.0049435787
    63  9A:9F:06:44:24:5B      WPA2       Long Huong Galaxy M12 0.0048118985
    64  96:FF:FC:91:EF:64 WPA3 WPA2                        <NA> 0.0046680498
    65  A6:02:B9:73:81:47      WPA2                C239U53 6056 0.0044981061
    66  0C:80:63:A9:6E:EE      WPA2                        <NA> 0.0043622767
    67  12:51:07:FF:29:D6      WPA2        DESKTOP-KITJO8R 5262 0.0042763597
    68  9E:A3:A9:DB:7E:01      WPA2                        <NA> 0.0041862899
    69  92:F5:7B:43:0B:69      WPA2                    Redmi 12 0.0040983607
    70  86:DF:BF:E4:2F:23      WPA2        DESKTOP-Q7R0KRV 2433 0.0040922619
    71  A6:02:B9:73:83:18      WPA2                C239U52 6706 0.0037142233
    72  E8:28:C1:DD:04:40       OPN               MIREA_HOTSPOT 0.0031914894
    73  26:20:53:0C:98:E8 WPA3 WPA2                        <NA> 0.0028708134
    74  E8:28:C1:DD:04:42       OPN                        <NA> 0.0027650878
    75  E8:28:C1:DD:04:41       OPN                MIREA_GUESTS 0.0026595745
    76  B6:C4:55:B5:53:24      WPA2                     Redmi 9 0.0023434884
    77  E8:28:C1:DD:04:50       OPN                MIREA_GUESTS 0.0022249416
    78  00:23:EB:E3:81:F1      WPA2                         IKB 0.0020325203
    79  E8:28:C1:DC:BD:50       OPN                MIREA_GUESTS 0.0018228217
    80  E8:28:C1:DD:04:51       OPN                        <NA> 0.0015948963
    81  02:67:F1:B0:6C:98       OPN                     MT_FREE 0.0015360983
    82  E8:28:C1:DC:C8:32       OPN               MIREA_HOTSPOT 0.0012558870
    83  E8:28:C1:DC:C8:31       OPN                        <NA> 0.0011112655
    84  E8:28:C1:DC:C6:B0       OPN                MIREA_GUESTS 0.0010311936
    85  00:26:CB:AA:62:71      WPA2                         IKB 0.0010157440
    86  9E:A3:A9:BF:12:C0      WPA2                        <NA> 0.0009708738
    87  E8:28:C1:DC:C8:30       OPN                MIREA_GUESTS 0.0008288928
    88  00:23:EB:E3:81:F2      WPA2                        GIVC 0.0007295466
    89  7E:3A:10:A7:59:4E      WPA2                    vivo Y21 0.0006506181
    90  E8:28:C1:DC:B2:41       OPN                MIREA_GUESTS 0.0006019020
    91  E8:28:C1:DC:C6:B1       OPN                        <NA> 0.0005959476
    92  E8:28:C1:DC:B2:42       OPN                        <NA> 0.0005751754
    93  E8:28:C1:DC:B2:40       OPN               MIREA_HOTSPOT 0.0005427703
    94  0A:24:D8:D9:24:70      WPA2                   IgorKotya 0.0004912798
    95  E8:28:C1:DE:74:31       OPN                        <NA> 0.0004511617
    96  EA:7B:9B:D8:56:34      WPA2                    POCO C40 0.0004462294
    97  E8:28:C1:DD:04:52       OPN               MIREA_HOTSPOT 0.0004091653
    98  10:50:72:00:11:08      WPA2              MGTS_GPON_B563 0.0004002401
    99  E8:28:C1:DE:47:D2       OPN               MIREA_HOTSPOT 0.0003318217
    100 EA:D8:D1:77:C8:08      WPA2     DIRECT-08-HP M428fdw LJ 0.0002002002
    101 E8:28:C1:DE:74:32       OPN               MIREA_HOTSPOT 0.0001926782
    102 56:99:98:EE:5A:4E      WPA2               tementy-phone 0.0001134945

#### Данные клиентов

##### 1. Определить производителя для каждого обнаруженного устройства

``` r
data_2 %>%
  filter(grepl("(..:..:..:)(..:..:..)", BSSID)) %>%
  distinct(BSSID)
```

                   BSSID
    1  BE:F1:71:D5:17:8B
    2  BE:F1:71:D6:10:D7
    3  1E:93:E3:1B:3C:F4
    4  E8:28:C1:DC:FF:F2
    5  00:25:00:FF:94:73
    6  00:26:99:F2:7A:E2
    7  0C:80:63:A9:6E:EE
    8  E8:28:C1:DD:04:52
    9  0A:C5:E1:DB:17:7B
    10 9A:75:A8:B9:04:1E
    11 8A:A3:03:73:52:08
    12 4A:EC:1E:DB:BF:95
    13 BE:F1:71:D5:0E:53
    14 08:3A:2F:56:35:FE
    15 6E:C7:EC:16:DA:1A
    16 2A:E8:A2:02:01:73
    17 E8:28:C1:DC:B2:52
    18 E8:28:C1:DC:C6:B2
    19 E8:28:C1:DC:C8:32
    20 56:C5:2B:9F:84:90
    21 9A:9F:06:44:24:5B
    22 12:48:F9:CF:58:8E
    23 E8:28:C1:DD:04:50
    24 AA:F4:3F:EE:49:0B
    25 3A:70:96:C6:30:2C
    26 E8:28:C1:DC:3C:92
    27 8E:55:4A:85:5B:01
    28 5E:C7:C0:E4:D7:D4
    29 E2:37:BF:8F:6A:7B
    30 96:FF:FC:91:EF:64
    31 CE:B3:FF:84:45:FC
    32 00:26:99:BA:75:80
    33 76:70:AF:A4:D2:AF
    34 E8:28:C1:DC:B2:50
    35 00:AB:0A:00:10:10
    36 E8:28:C1:DC:C8:30
    37 8E:1F:94:96:DA:FD
    38 E8:28:C1:DB:F5:F2
    39 E8:28:C1:DD:04:40
    40 EA:7B:9B:D8:56:34
    41 BE:FD:EF:18:92:44
    42 7E:3A:10:A7:59:4E
    43 00:26:99:F2:7A:E1
    44 00:23:EB:E3:49:31
    45 E8:28:C1:DC:B2:40
    46 E0:D9:E3:49:04:40
    47 3A:DA:00:F9:0C:02
    48 E8:28:C1:DC:B2:41
    49 E8:28:C1:DE:74:32
    50 E8:28:C1:DC:33:12
    51 92:F5:7B:43:0B:69
    52 DC:09:4C:32:34:9B
    53 E8:28:C1:DC:F0:90
    54 E0:D9:E3:49:04:52
    55 22:C9:7F:A9:BA:9C
    56 E8:28:C1:DD:04:41
    57 92:12:38:E5:7E:1E
    58 B2:1B:0C:67:0A:BD
    59 E8:28:C1:DC:33:10
    60 E0:D9:E3:49:04:41
    61 1E:C2:8E:D8:30:91
    62 A2:64:E8:97:58:EE
    63 A6:02:B9:73:2F:76
    64 A6:02:B9:73:81:47
    65 AE:3E:7F:C8:BC:8E
    66 B6:C4:55:B5:53:24
    67 86:DF:BF:E4:2F:23
    68 02:67:F1:B0:6C:98
    69 36:46:53:81:12:A0
    70 E8:28:C1:DC:0B:B0
    71 82:CD:7D:04:17:3B
    72 E8:28:C1:DC:54:B2
    73 00:03:7F:10:17:56
    74 00:0D:97:6B:93:DF

00:03:7F Atheros Communications Inc.  00:0D:97 Hitachi Energy USA Inc. 
00:23:EB Cisco Systems Inc 00:25:00 Apple Inc.  00:26:99 Cisco Systems
Inc 08:3A:2F Guangzhou Juan Intelligent Tech Joint Stock Co. Ltd
0C:80:63 Tp-Link Technologies Co.Ltd. DC:09:4C Huawei Technologies
Co.Ltd E0:D9:E3 Eltex Enterprise Ltd.  E8:28:C1 Eltex Enterprise Ltd.

##### 2. Обнаружить устройства, которые НЕ рандомизируют свой MAC адрес

``` r
data_2 %>%
  filter(grepl("(..:..:..:)(..:..:..)", BSSID) & !is.na(Probed.ESSIDs)) %>%
  select(BSSID, Probed.ESSIDs) %>%
  group_by(BSSID, Probed.ESSIDs) %>%
  filter(n() > 1) %>%
  arrange(BSSID) %>%
  unique()
```

    # A tibble: 16 × 2
    # Groups:   BSSID, Probed.ESSIDs [16]
       BSSID             Probed.ESSIDs   
       <chr>             <chr>           
     1 00:26:99:BA:75:80 GIVC            
     2 00:26:99:F2:7A:E2 GIVC            
     3 0C:80:63:A9:6E:EE KOTIKI_XXX      
     4 1E:93:E3:1B:3C:F4 Galaxy A71      
     5 6E:C7:EC:16:DA:1A Cnet            
     6 8E:55:4A:85:5B:01 Vladimir        
     7 AA:F4:3F:EE:49:0B Redmi Note 8 Pro
     8 BE:F1:71:D5:17:8B C322U13 3965    
     9 E8:28:C1:DC:B2:50 MIREA_HOTSPOT   
    10 E8:28:C1:DC:B2:50 MIREA_GUESTS    
    11 E8:28:C1:DC:B2:52 MIREA_HOTSPOT   
    12 E8:28:C1:DC:C6:B2 MIREA_HOTSPOT   
    13 E8:28:C1:DC:F0:90 MIREA_GUESTS    
    14 E8:28:C1:DD:04:40 MIREA_HOTSPOT   
    15 E8:28:C1:DD:04:52 MIREA_HOTSPOT   
    16 E8:28:C1:DE:74:32 MIREA_HOTSPOT   

##### 3. Кластеризовать запросы от устройств к точкам доступа по их именам. Определить время появления устройства в зоне радиовидимости и время выхода его из нее.

``` r
cluster_data <- data_2 %>%
  filter(!is.na(Probed.ESSIDs), !is.na(Power), !is.na(First.time.seen), !is.na(Last.time.seen)) %>% 
  group_by(Station.MAC, Probed.ESSIDs) %>%
  summarise(
    "first_time" = min(First.time.seen),
    "last_time" = max(Last.time.seen),
   Power = ifelse(all(is.numeric(as.numeric(Power))), sum(as.numeric(Power)), NA)
  ) %>%
  arrange(first_time)
```

    `summarise()` has grouped output by 'Station.MAC'. You can override using the
    `.groups` argument.

``` r
cluster_data
```

    # A tibble: 1,477 × 5
    # Groups:   Station.MAC [1,477]
       Station.MAC       Probed.ESSIDs first_time          last_time           Power
       <chr>             <chr>         <dttm>              <dttm>              <dbl>
     1 5C:3A:45:9E:1A:7B C322U21 0566  2023-07-28 09:13:03 2023-07-28 11:51:54   -39
     2 96:35:2D:3D:85:E6 IT2 Wireless  2023-07-28 09:13:03 2023-07-28 09:13:03   -65
     3 C0:E4:34:D8:E7:E5 C322U13 3965  2023-07-28 09:13:03 2023-07-28 11:53:16   -61
     4 CA:66:3B:8F:56:DD C322U13 3965  2023-07-28 09:13:03 2023-07-28 10:59:44   -33
     5 68:54:5A:40:35:9E C322U06 5179  2023-07-28 09:13:06 2023-07-28 11:50:50   -31
     6 CA:54:C4:8B:B5:3A GIVC          2023-07-28 09:13:06 2023-07-28 11:55:04   -65
     7 4A:C9:28:46:EE:3F KOTIKI_XXX    2023-07-28 09:13:08 2023-07-28 11:36:29   -65
     8 A0:E7:0B:AE:D5:44 AndroidAP177B 2023-07-28 09:13:09 2023-07-28 11:34:42   -37
     9 00:95:69:E7:7C:ED nvripcsuite   2023-07-28 09:13:11 2023-07-28 11:56:13   -55
    10 00:95:69:E7:7F:35 nvripcsuite   2023-07-28 09:13:11 2023-07-28 11:56:07   -69
    # ℹ 1,467 more rows

##### 4. Оценить стабильность уровня сигнала внури кластера во времени. Выявить наиболее стабильный кластер

``` r
stability_level <- cluster_data %>%
  group_by(Station.MAC, Probed.ESSIDs) %>%
  summarise(Mean_Power = mean(Power)) %>%
  arrange((Mean_Power))
```

    `summarise()` has grouped output by 'Station.MAC'. You can override using the
    `.groups` argument.

``` r
stability_level %>% head(1)
```

    # A tibble: 1 × 3
    # Groups:   Station.MAC [1]
      Station.MAC       Probed.ESSIDs  Mean_Power
      <chr>             <chr>               <dbl>
    1 8A:45:77:F9:7F:F4 iPhone (Дима )        -89

## Оценка результата

В результате лабораторной работы были выполнены задания по анализу
данных трафика Wi-Fi сетей

## Вывод

В ходе лабораторной работы были импортированы и проанализированы данные
трафика Wi-Fi сетей
