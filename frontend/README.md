#  Проект «Kittygram»  
  
> Проект Kittygram позволяет пользователям поделиться своим пушистым другом
  
## У нас вы можете:  
- #### Зарегистрироваться.  
- #### Добавить кота!  
- #### Убрать (своего) кота!  
- #### Подобрать цвет!  
- #### Установить фото любимца!  
- #### Рассказть всем о достижениях котика!  
- #### Указать возраст котика.  

## Как развернуть проект локально  
  
Клонировать репозиторий и перейти в него в командной строке:    
    
```  
git clone https://github.com/andrey-kobelev/kittygram_final.git
```    
    
```  
cd kittygram_final
```    
    
Cоздать и активировать виртуальное окружение:    
    
```  
python3 -m venv venv  
```    
    
```  
source venv/bin/activate  
```    
    
Установить зависимости из файла requirements.txt:    
    
```  
python3 -m pip install --upgrade pip  
```    
    
```  
pip install -r requirements
```    

## Запустить проект локально

Файл `.env`, для локального запуска должен быть таким и находиться в корневой директории проекта:

```
POSTGRES_USER=django_user  
POSTGRES_PASSWORD=123456789django  
POSTGRES_DB=django_kitty
DB_HOST=db  
DB_PORT=5432  
SECRET_KEY=<django_secret_key_from_settings> 
ALLOWED_HOSTS=<domain>,<server_ip>,127.0.0.1,localhost,0.0.0.0  
DEBUG_VALUE=True  
SQLITE=True
```

Далее в корневой директории выполните команду:

```
sudo docker compose -f docker-compose.yml up -d --build
```

> В проекте два файла docker compose: один для продакшена другой для локального запуска. `docker-compose.yml` - для локального запуска.

Далее поочередно выполните следующие команды так же находясь в корневой директории проекта:

```
sudo docker compose -f docker-compose.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```

Чтобы остановить контейнеры:

```
sudo docker compose -f docker-compose.yml down
```

## Как запустить проект на сервере:  
Настраиваем Docker

```bash
sudo apt update  
sudo apt install curl  
curl -fSL https://get.docker.com -o get-docker.sh  
sudo sh ./get-docker.sh  
sudo apt-get install docker-compose-plugin;  
# Переходим в директорию проекта.  
cd kittygram_final/  
# Создаем и редактируем файл .env, в котором нужно указать данные  
sudo nano .env  
# DJANGO_KEY=django-insecure-cg6*%6d51ef8f#4!r3*$vmxm4)abgjw8mo!4y-q*uq1!4$-88$  
POSTGRES_DB=<Желаемое_имя_базы_данных>  
POSTGRES_USER=<Желаемое_имя_пользователя_базы_данных>  
POSTGRES_PASSWORD=<Желаемый_пароль_пользователя_базы_данных>  
DB_HOST=db  
DB_PORT=5432  
ALLOWED_HOSTS=<domain>,<server_ip>,127.0.0.1,localhost,0.0.0.0
# Сохраняем файл. Ключ DJANGO_KEY рекомендуется заменить  
# Далее выполняем последовательно  
sudo docker compose -f docker-compose.production.yml pull  
sudo docker compose -f docker-compose.production.yml down  
sudo docker compose -f docker-compose.production.yml up -d  
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate  
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic  
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/   
```  
  
Устанавливаем и настраиваем NGINX  
  
```bash  
# Устанавливаем NGINX  
sudo apt install nginx -y  
# Запускаем  
sudo systemctl start nginx  
# Настраиваем firewall  
sudo ufw allow 'Nginx Full'  
sudo ufw allow OpenSSH  
# Включаем firewall  
sudo ufw enable  
# Открываем конфигурационный файл NGINX  
sudo nano /etc/nginx/sites-enabled/default  
# Полностью удаляем из него все и пишем новые настройки  
  
server {  
    listen 80;    server_name example.com;        location / {  
        proxy_set_header HOST $host;        proxy_pass http://127.0.0.1:9000;  
    }}  
  
# Сохраняем изменения и выходим из редактора  
# Проверяем корректность настроек  
sudo nginx -t  
# Запускаем NGINX  
sudo systemctl start nginx  
```  
Настраиваем HTTPS  
  
```bash  
# Установка пакетного менеджера snap.  
# У этого пакетного менеджера есть нужный вам пакет — certbot.  
sudo apt install snapd  
# Установка и обновление зависимостей для пакетного менеджера snap.  
sudo snap install core; sudo snap refresh core  
# При успешной установке зависимостей в терминале выведется:  
# core 16-2.58.2 from Canonical✓ installed   
# Установка пакета certbot.  
sudo snap install --classic certbot  
# При успешной установке пакета в терминале выведется:  
# certbot 2.3.0 from Certbot Project (certbot-eff✓) installed  
  
# Создание ссылки на certbot в системной директории,  
# чтобы у пользователя с правами администратора был доступ к этому пакету.  
sudo ln -s /snap/bin/certbot /usr/bin/certbot  
  
# Получаем сертификат и настраиваем NGINX следуя инструкциям  
sudo certbot --nginx  
# Перезапускаем NGINX  
sudo systemctl reload nginx  
```  
  


## В проекте были использованы технологии:  
* #### Django REST  
* #### Python 3.9  
* #### Gunicorn  
* #### Nginx  
* #### JS  
* #### Node.js  
* #### PostgreSQL  
* #### Docker  
---  
### Над проектом работал:  
* [Kobelev Andrey](https://github.com/andrey-kobelev)