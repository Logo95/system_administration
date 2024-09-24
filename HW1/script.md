
echo 'топ 10 - ip адресов' >> HW1.txt
grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log | sort | uniq -c | sort -nr | head -10 >> HW1.txt

echo 'подсчет количества методов POST, GET, PUT, DELETE, PATCH:' >> HW1.txt
grep -Eo 'GET|POST|PUT|DELETE|PATCH' access.log | sort | uniq -c >> HW1.txt

echo 'количество операционных систем, с которых выполняются запросы в порядке убывания:' >> HW1.txt
grep -Eo '\((Windows|Linux|Macintosh|Android|iPhone)[^)]*\)' access.log | sort | uniq -c | sort -nr >> HW1.txt

echo 'Самое популярное устройство по количеству сделанных запросов (наименование запросов):' >> HW1.txt
grep -Eo '\(Linux|Android|iPhone|Windows|Macintosh|Other)' access.log | sort | uniq -c | sort -nr | head -1 >> HW1.txt

echo 'перечисление пяти самых популярных браузеров включая их версию (как в заголовке):' >> HW1.txt
grep -Eo '(Mozilla|Chrome|Safari|Opera|Edge)[^"]+' access.log | sort | uniq -c | sort -nr | head -5 >> HW1.txt