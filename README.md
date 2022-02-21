# Ansible Basics_Practice Task 3
## Задание
### 1. Сделайте роль для установки Apache, используя плейбук из первого домашнего задания. Требования те же:
   - установка пакета httpd;
   - включение службы веб-сервера и проверка, что он запущен;
   - создание файла /var/www/html/index.html;
   - открытие необходимых для работы веб-сервера портов брендмауэра.  

    Хотелось бы увидеть использование переменных (в том числе фактов), хендлеров, шаблонов и т.п., что мы рассматривали в рамках нашего курса.
### 2. Напишите роль, которая устанавливает и включает FTP (пакет vsftpd), открывает необходимые порты. Определите в переменных необходимые параметры конфигурации ftp-сервера и используйте их в шаблоне для файла конфигурации vsftpd.conf:
   - разрешен анонимный доступ в папку /var/ftp/pub и аплоад файлов в папку /var/ftp/pub/upload;
   - настроены необходимые разрешения и соответствующий SELinux контекст: "ftpd_anon_write" boolean - значение "on" (edited).
## Результаты
### Apacher Results
#### Доступные опции
```yml
http_port: 8080 # Возможна настройка порта, на котором необходимо запустить сервис
```
#### Визуальная проверка работоспособности
![HTTP node 1](images/HTTP_node1.png)
![HTTP node 2](images/HTTP_node2.png)
### Ftp Result
#### Доступные опции
```yml
ftpd_banner: "Welcome to My FTP service." # Возможно изменение приветствия
ftp_folders: # Можно задать список папок, к которым необходимы права на запись
  - upload
```
#### Визуальная проверка работоспособности
![HTTP node 2](images/FTP.png)