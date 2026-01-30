# CI/CD

Задача, сделать автоматический простой деплой на [[VPS]], после коммита.

После пуша коммита на github, система должна подключиться к VPS, собрать образ [[Docker]], остановить контейнер (если есть) и запустить новый контейнер на основе собранного образа.

>Прим.: если надо сделать коммит без запуска Actions, то самый простой способ это в комментарий добавить `[skip ci]`. Выглядит так: `git commit -m "bla-bla [skip ci]"`

### 1. Создать проект

Здесь писать ничего не буду. Создаём простой проект на [[NodeJS]] для теста.

### 2. Создать нового пользователя на VPS для деплоя

Подключиться к VPS под root-ом:

```bash
# пользователь не использует пароль
adduser --disabled-password deployer

# добавить deployer в группу docker
usermod -aG docker deployer
```

Т.к. пользователь не использует пароль, надо прописать [[Доступ к серверу по SSH|ssh-ключ]] со своей хост-машины.

```bash
# флаг -p создаёт промежуточные каталоги
mkdir -p /home/deployer/.ssh
# прописать публичный ключ
nano /home/deployer/.ssh/authorized_keys
# изменить владельца каталога на deployer
chown -R deployer:deployer /home/deployer/.ssh
# даёт полные права (чтение/запись/вход)
chmod 700 /home/deployer/.ssh
# даёт права на чтение и запись только владельцу файла authorized_keys
chmod 600 /home/deployer/.ssh/authorized_keys
```

### 3. Создать ssh-ключ для github

Подключиться к [[VPS]] под пользователем `deployer` и в корневой директории выполнить:

```bash
ssh-keygen -t ed25519 -f github_actions_key -N ""
# -t ed25519 - тип ключа
# -f github_actions_key - имя файла для ключа
# -N "" - парольная фраза — в данном примере пустая
# -C "comment" - комментарий
```

После этого в директории появятся 2 файла. Дальше надо записать публичный ключ в `~/.ssh/authorized_keys`, второй отправить на github (читай про добавление секретов).

```bash
cat github_actions_key.pub >> ~/.ssh/authorized_keys
```

### 4. Добавить секреты в github

Зайди в репозиторий на `GitHub` → `Settings` → `Secrets and variables` → `Actions`

Нажать `New repository secret` и добавить два секрета:

- `VPS_HOST` - IP-адрес VPS
- `SSH_PRIVATE_KEY` - содержимое приватного ключа github_actions_key

>Прим.: схема подключения выглядит так: публичный ключ у VPS, приватный ключ у github. Наоборот не сработает.

### 5. Добавить файл с пайплайном

В проекте создать файл с пайплайном `./github/workflows/deploy.yml`

```yml
name: Deploy to VPS
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    # 1. Забрать код
    - name: Checkout code
      uses: actions/checkout@v3
      with:
	    ref: main
      
    # 2. Настроить SSH
    - name: Setup SSH
      uses: webfactory/ssh-agent@v0.7.0
      with:
        ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

    # 3. Деплой на VPS
    - name: Deploy to VPS
      run: |
        ssh -o StrictHostKeyChecking=no deployer@${{ secrets.VPS_HOST }} "
          # Создаём папку, если её нет
          mkdir -p ~/app
          cd ~/app

          # Если это не git-репозиторий — клонируем
          if [ ! -d .git ]; then
            git clone https://github.com/GeoS74/test-cicd.git .
          fi

          git pull origin main &&
          docker build -t app:0.0.1 . &&
          docker stop myapp-container || true &&
          docker rm myapp-container || true &&
          docker run -d \
            --name myapp-container \
            --restart unless-stopped \
            -p 3000:3000 \
            app:0.0.1
        "
```

##### Небольшие пояснения:

`runs-on: ubuntu-latest` - это система, на которой запускается GitHub Actions, а не VPS. Поэтому версию не меняем.

```
name: Checkout code
uses: actions/checkout@v3
```
Скачивает код репозитория внутрь виртуальной машины GitHub Actions и переходит на ветку `main`. Ветку выберет исходя из `branches: [ main ]`, но можно указать вручную:
```
- name: Checkout code
  uses: actions/checkout@v3
  with:
	  ref: main
```

### 6. Добавляем тестирование перед деплоем

Файл с пайплайном `./github/workflows/deploy.yml` теперь такой:

```yml
name: Deploy to VPS
on:
  push:
    branches: [ main ]

jobs:
  # добавить этот блок
  test:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '19'
        cache: 'npm'
    - name: Install dependencies
      run: npm ci
    - name: Run tests
      run: npm test

  deploy:
    runs-on: ubuntu-latest
    needs: test  # Запустится только если test job успешен
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      with:
        ref: main

    - name: Setup SSH
      uses: webfactory/ssh-agent@v0.7.0
      with:
        ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

    - name: Deploy to VPS
      run: |
        ssh -o StrictHostKeyChecking=no deployer@${{ secrets.VPS_HOST }} "
          # Создаём папку, если её нет
          mkdir -p ~/app
          cd ~/app
          # Если это не git-репозиторий — клонируем
          if [ ! -d .git ]; then
            git clone https://github.com/GeoS74/test-cicd.git .
          fi
          git pull origin main &&
          docker build -t app:0.0.1 . &&
          docker stop myapp-container || true &&
          docker rm myapp-container || true &&
          docker run -d \
            --name myapp-container \
            --restart unless-stopped \
            -p 3000:3000 \
            app:0.0.1
        "
```

##### Небольшие пояснения:

`cache: 'npm'` - кеширует папку `node_modules` между запусками workflow.

Как работает:
1. При первом запуске `npm ci` сохраняет зависимости в кеш GitHub Actions
2. При следующих запусках зависимости берутся из кеша, а не устанавливаются заново
3. Ускоряет выполнение job (экономит 30-60 секунд)


`npm ci` предпочтительнее для CI/CD:
1. Чистая установка - удаляет `node_modules` перед установкой
2. Использует `package-lock.json` - устанавливает точные версии, гарантирует повторяемость
3. Быстрее для чистых инсталляций
4. Строгий - если `package-lock.json` не синхронизирован с `package.json`, падает с ошибкой

`npm i` - для локальной разработки, `npm ci` - для production/CI.


#ci/cd