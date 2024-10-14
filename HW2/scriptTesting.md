--делаем файл исполняемым

chmod +x serverTest

--запускаем сервер

./serverTest

--проверяем на наличие зомби-процессов

watch -n 1 'ps aux | grep Z'
```
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
logo         979  0.0  0.0      0     0 ?        Z    08:39   0:00 [pulseaudio] <defunct>
logo        6321  0.0  0.0   6360  1036 pts/1    S+   09:00   0:00 grep --color=auto Z
```
--Количество открытых файловых дескрипторов

go run -race test1.go
```
Ошибка открытия файла: open /dev/null: too many open files
signal: killed
```

--Многократные запросы

```
for i in {1..15}; do curl http://localhost:8080/bugs; done
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Internal Server Error
Server refuses to handle requests after 10th cal
```
