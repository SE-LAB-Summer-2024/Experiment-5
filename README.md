# Notes project

## Requirements
- Python3
- Postgres

## How to run

### Setup virtual environment

#### Create venv
```
python -m venv ./venv
```

#### Install requirements
```
python -m pip install -r requirements.txt
```

#### Activate venv
```
source ./venv/bin/activate
```

### Setup database
1. Create an instance of postgres database
2. Make migrations
    ```
    python manage.py makemigrations
    ```
3. Migrate
    ```
    python manage.py migrate
    ```

### Create an admin
```
python manage.py createsuperuser
```

## Important end-points
```
users/login/ --> login a user
users/me/ --> get information of logged-in user
users/create/ --> create a user
users/<id>/delete/ --> delete a user
notes/ --> list all notes of current user
notes/<id>/ --> get details of a note
notes/create/ --> create a note
notes/<id>/delete/ --> delete a note
```


-------------------
کدی که در docker-compose قرار گرفت :‌

```
version: '3.8'

services:
  db:
    image: docker.arvancloud.ir/postgres:15
    environment:
      POSTGRES_DB: postgres
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: 12345678
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - django-network

  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
    environment:
      - DATABASE_NAME=postgres
      - DATABASE_USER=postgres
      - DATABASE_PASSWORD=12345678
      - DATABASE_HOST=db
    networks:
      - django-network

volumes:
  postgres_data:

networks:
  django-network:
```

### توضیحات مربوطه :‌

نسخه: نسخه‌ی '3.8' مشخص‌کننده‌ی نسخه‌ی فرمت فایل Docker Compose است.

سرویس‌ها:

###  db:

Image:

 از تصویر رسمی PostgreSQL postgres:13 استفاده می‌کند.

Environment Variables:

 تنظیمات نام دیتابیس، کاربر، و رمز عبور را انجام می‌دهد.

Volumes:

 داده‌های PostgreSQL را بین ریستارت‌های کانتینر با استفاده از یک حجم نام‌گذاری‌شده postgres_data حفظ می‌کند.

Networks:

 به شبکه‌ی django-network متصل می‌شود که امکان ارتباط بین برنامه‌ی Django و PostgreSQL را فراهم می‌کند.

### web:
Build: 

تصویر Docker را از Dockerfile در دایرکتوری جاری می‌سازد.


Command:

 سرور توسعه‌ی Django را اجرا می‌کند.


Volumes:

 دایرکتوری جاری را در کانتینر در مسیر /code متصل می‌کند.


Ports:

 پورت 8000 در میزبان را به پورت 8000 در کانتینر متصل می‌کند.


Depends_on:

 اطمینان حاصل می‌کند که کانتینر PostgreSQL db قبل از کانتینر Django شروع به کار می‌کند.


Environment:

 متغیرهای محیطی مورد نیاز برای اتصال Django به PostgreSQL را تنظیم می‌کند.


Networks:

 به همان شبکه‌ی django-network که دیتابیس در آن قرار دارد متصل می‌شود.

Volumes:

 یک حجم نام‌گذاری‌شده به نام postgres_data برای ذخیره‌ی داده‌های PostgreSQL تعریف می‌کند.

Networks:

 یک شبکه‌ی سفارشی به نام django-network تعریف می‌کند تا ارتباط بین سرویس‌ها را امکان‌پذیر سازد.