# dz2
1) Добавляем репозиторий докера (чтобы получить последнюю его версию)
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg  lsb-release -y
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
2) Устанавливаем docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
3) Устанавливаем docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
4) Проверяем, что всё установилось
sudo docker -v
sudo docker ps
sudo docker images
и для docker-compose
docker-compose -v
========================================
Докер готов

-- 1. Создаем docker-сеть:
sudo docker network create pg-net

-- 2. подключаем созданную сеть к контейнеру сервера Postgres:
sudo docker run --name pg-server --network pg-net -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 -v /var/lib/postgres:/var/lib/postgresql/data postgres:15
-- 3. Запускаем отдельный контейнер с клиентом в общей сети с БД:
sudo docker run -it --rm --network pg-net --name pg-client postgres:15 psql -h pg-server -U postgres
вводим пароль postgres
