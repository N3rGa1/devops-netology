# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис

### Ответ
```
1: { "info" : "Sample JSON output from our service\t",
 2:     "elements" :[
 3:         { "name" : "first",
 4:         "type" : "server",
 5:         "ip" : 7175 
 6:         },
 7:         { "name" : "second",
 8:         "type" : "proxy",
 9:         "ip": "71.78.22.43"
10:         }
11:     ]
12: }
```

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python


#!/usr/bin/env python

import socket as s
import time as t
import datetime as dt
import json
import yaml

# set variables
i     = 1
wait  = 2
srv   = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}
init  = 0
# start script workflow
print('*** start script ***')
print(srv)
print('********************')

while 1 == 1 :
  for host in srv:
    is_error = False
    ip = s.gethostbyname(host)
    if ip != srv[host]:
      if i==1 and init !=1:
                print(str(dt.datetime.now().strftime("%Y-%m-%d %H:%M:%S")) +' [ERROR] ' + str(host) +' IP mistmatch: '+srv[host]+' '+ip)

# json
with open(‘/home/vagrant/log.json’,'w') as jsf:
          json_data= json.dumps({host:ip})
          jsf.write(json_data)
# yaml
with open(‘/home/vagrant/lo.yaml’,'w') as ymf:
          yaml_data= yaml.dump([{host : ip}])
          ymf.write(yaml_data)
srv[host] = ip
i+=1
if i <= 50:
t.sleep(wait)
```


### Вывод скрипта при запуске при тестировании:
```
vagrant@vagrant:~$ python3.8 1.py
*** start script ***
{'drive.google.com': '0.0.0.0', 'mail.google.com': '0.0.0.0', 'google.com': '0.0.0.0'}
********************
2022-03-21 08:20:49 [ERROR] drive.google.com IP mistmatch: 0.0.0.0 64.233.164.194
2022-03-21 08:20:49 [ERROR] mail.google.com IP mistmatch: 0.0.0.0 64.233.165.17
2022-03-21 08:20:49 [ERROR] google.com IP mistmatch: 0.0.0.0 108.177.14.139

```

### json-файл(ы), который(е) записал ваш скрипт:
```json
["2022-03-21 08:20:49 [ERROR] drive.google.com IP mistmatch: 0.0.0.0 64.233.164.194","2022-03-21 08:20:49 [ERROR] mail.google.com IP mistmatch: 0.0.0.0 64.233.165.17","2022-03-21 08:20:49 [ERROR] google.com IP mistmatch: 0.0.0.0 108.177.14.139"]

```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
---
- '2022-03-21 08:20:49 [ERROR] drive.google.com IP mistmatch: 0.0.0.0 64.233.164.194'
- '2022-03-21 08:20:49 [ERROR] mail.google.com IP mistmatch: 0.0.0.0 64.233.165.17'
- '2022-03-21 08:20:49 [ERROR] google.com IP mistmatch: 0.0.0.0 108.177.14.139'

```

