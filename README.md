 * Для запуска должен быть установлен Docker и Ansible

 * Для работы Elasticsearh нужно изменить значение vm.max_map_count, на сервере где он будет запускаться

 - Проверить
 `more /proc/sys/vm/max_map_count`

 - Одноразово применить, сбросится после перезагрузки
 `sudo sysctl -w vm.max_map_count=262144`

 - Записать в файл и применить
 `sudo sh -c 'echo "vm.max_map_count = 262144" >> /etc/sysctl.conf'`
 `sudo sysctl -p`

## Запуск

 - Запуска для поднятия ELK+fealbeat

`sudo ansible-playbook -i inventory/hosts.yml --ask-pass elk.yml`

 * если нужно запустить что то конкретное использвать теги

`sudo ansible-playbook -i inventory/hosts.yml --ask-pass -t elasticsearch,kibana elk.yml`

`
tags: ["docker"] }
tags: ["elasticsearch"] }
tags: ["kibana"] }
tags: ["logstash"] }
tags: ["filebeat"] }
tags: ["nginx"] }
`
* Для остановки контейнеров, можно также использовать теги

`ansible-playbook -i inventory/hosts.yml elk-stop.yml`

### Kibana
 * Адрес для подключения Kibana

 - http://localhost

  Логин: elastic
  Пароль: changeme

 * После входу нужно созать Index

 `Stack Management - > Index patterns -> Create index pattern -> Вписать filebeat-* -> next step -> @timestamp -> Create index pattern`

Проверка node в кластере
 Dev Tools -> `GET /_cat/nodes?v=true&h=id,ip,port,v,m`
