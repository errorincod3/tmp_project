
Redis — это база данных, размещаемая в памяти, которая используется, в основном, в роли кеша, находящегося перед другой, «настоящей» базой данных, вроде MySQL или PostgreSQL. Кеш, основанный на Redis, помогает улучшить производительность приложений.

```bash
brew services stop redis
brew services start redis
brew services status redis

systemctl startredis
systemctl stop redis
systemctl status redis
systemctl disable redis 
redis-cli
```

### Команды

```bash
set - добавление
get - получение
getset - изменение значения
type - тип значения
rename - измение ключа
exists - существует ключ или нет
append name text - добавление в конец значения
del - удаление
flushdb - удаление базы
expire name 30 - временное создание пары
ttl name - получение время до удаление пары
keys * - вывод всех ключей (воздержаться от использования)
keys name* - поиск по имени ключа
incr name - увеличение на одно значение
decr - уменьшение на одно значени
incrby name - увеличение на заданное значене
decrby - уменьшение на заданное значене
```

### Безопасноть

```bash
vim /etc/redis/redis.conf - конфигурация
```

supervised systemd

```bash
supervised systemd
bind 127.0.0.1 ::1 - привязка к localhost
requirepass foobared - аунтетификация по паролю
```

### Трансзакции

```bash
redis 127.0.0.1:6379> multi                 
OK                                          
redis 127.0.0.1:6379> set test:1:trValue "1"
QUEUED                                      
redis 127.0.0.1:6379> incr test:1:trValue   
QUEUED                                      
redis 127.0.0.1:6379> decr test:1:trValue   
QUEUED                                      
redis 127.0.0.1:6379> exec                  
1) OK                                       
2) (integer) 2                              
3) (integer) 1        
```

### Строки числа

```bash
append test:1:string " world!"  - добавит в конец world
strlen test:1:string - вывод количества символов
getrange test:1:string 6 10  - вывод промежутка между 6 и 10
setrange test:1:string 6 "habrahabr!" - заменит после 6 символа 
```



### Операции над списком

```bash
lpush test:1:messages "Hello, world!" - добавляет в начало списка
rpush test:1:messages "Hello, world!"
rpush test:1:messages "Hello, user!" - добавляет в конец
rpush test:1:messages "Wow!"  
lrange test:1:messages 0 2
lpop test:1:messages  - удаляет первое значение
rpop - удаляет последнее значение    
llen test:1:messages - показывает длину списка
```



### Множества

```bash
sadd test:1:fruits "banana"                                                                                                              
sadd test:1:fruits "apple"                                                                                                                    
sadd test:1:fruits "strawberry"                                                                                                          
sadd test:1:yellowThings "banana"                                                                                                        
sadd test:1:yellowThings "apple"                                                                                                           
sadd test:1:redThings "strawberry" 

scard test:1:fruits - сколько элементов в множестве          
sunion - вывод элементов 
sdiff test:1:fruits test:1:yellowThings  - выведет значения                

sdiffstore test:1:noYellowFruits test:1:fruits test:1:yellowThings - вывод отсутсвующего ключ

sinter test:1:yellowThings test:1:fruits - вывод одинаковых элементов

sismember test:1:fruits "banana" - проверка существует значение к ключе

smove test:1:yellowThings test:1:redThings "apple" - перемещение apple из yellowThings в redThings
smembers test:1:redThings - рандомный вывод

spop test:1:redThings - убирает рандомно значение из списка

srandmember test:1:fruits - выводит рандомное значение из множества

srem test:1:fruits "banana" - проверка значение в множестве
                                               
```

### Упорядоченное множество 

```bash
zadd hackers 1912 "Alan Turing"
zadd hackers 1969 "Linus Torvalds"    
zadd hackers 1916 "Claude Shannon"
zadd hackers 1965 "Yukihiro Matsumoto"
zrange hackers 0 -1 - сортирует множества по возрастанию
zrevrange hackers 0 -1 - наоборот
zrangebyscore hackers -inf 1950 - выведет результат по 1950
zremrangebyscore hackers 1940 1960  - удаляет множества между 1940 и 1950 
```

### Хэш таблицы

```bash
hset users:1 name "Andrew"
hset users:1 email "andrew@example.com"
hkeys users:1  - вывод ключей
hvals users:1  - вывод значения
hget name name - вывод поля
hgetall users:1 - вывод всего
hincrby users:1 test 123  - увеличиваем значение на 123
hdel users:1 test - удаляем ключ со значением
```

### Pub/Sub, сообщения в Redis

```
SUBSCRIBE messages  
PUBLISH messages "Hello world!"

PSUBSCRIBE news.* - в первом подключении создается подписка news.*

PUBLISH news.art "New picture!" 
PUBLISH news.cinema "New movie!" - во втором подключении публикация 
```


```bash
rm -rf /
```
