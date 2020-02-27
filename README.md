# Отчет о выполнении Инженерной задачи Финала Олимпиады КД НТИ по профилю Информационная безопасность
---
### I. Исследование сетевой инфраструктуры

При первичном осмотре интерес представляет интерфейс `ens224`. Для его прослушки используем tshark на удаленной машине для захвата данных и Wireshark для их анализа. 
```
$ ssh root@45.80.67.89 -p 10102 'tshark -i ens224 -f "port !22" -w -' | wireshark -k -i -
```

Были обнаружены несколько хостов:

- `10.0.2.12`: - роутер, открытые порты: `22`. 
- `10.0.2.13`: роутер, открытые порты: `22, 53, 80, 8888`. Предположительно, управление вредоносом осуществляется по порту `8888`.
- `10.0.2.14`: роутер, открыты порты `53, 9999`. Порт `9999` может являться портом управления вредоносом.
- `192.168.1.2`: судя по трафику, является управляющим. Адрес явно указан в бинарных файлах первого и второго задания (MIPS и ARM). Управляющий сервер для хоста `10.0.2.12` (MIPS) запущен на порте `5555`.
### 2. MIPS

Поддерживает команды: wget, ping, проверка статуса.

###  Дополнительное задание

Был дан архив `task4.zip`. При иследовании с помощью `binwalk` в нем обанружилось 5 файлов. Но при распаковке (`unzip task4.zip`) а выходе было только 4 файла. Пробелма была решена с помощью команды `zip -FF task4.zip --out fix.zip`. После распаковки испарвленного архива удалось получить недостающий файл, но он, в отличии от других, содержит бинарные данные, преположительно зашифрованные. 

