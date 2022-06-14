# Desafio Docker KebDev

#### Build e Exec da aplicação rotten-potatoes.

> **Nota:** Criar o arquivo Dockerfile doretório SRC do projeto.

Apos criar o arquivo abra o mesmo e add as linhas abaixo para build da imagem:

```
FROM python:3.8.13-slim-buster
LABEL MAINTAINER = "Anderson Amaral - INICIATIVA KUBERNETES"

WORKDIR /app
COPY requirements.txt .
RUN python -m pip install -r requirements.txt
COPY . /app

EXPOSE 5000
CMD ["gunicorn", "--workers=3", "--bind", "0.0.0.0:5000", "-c","config.py", "app:app"]
```

Salve e feche o mesmo. 
Agora crie um **docker-compose.yml**, no compose vamos usar variáveis para facilitar concentrar a edição em um lugar, as declarações ficam no arquivo **.env** localizado dentro de *SRC*.

```
version: '3.8'

# Deployment da app web Rotten Potatoes
services:
  rotten-potatoes:
    build:
      dockerfile: Dockerfile
      context: ./
    privileged: true
    restart: always
    container_name: ${CONTAINER_NAME}
    hostname: ${APP_NAME}
    ports:
      - 8081:${PORT_APP}
    expose:
      - ${PORT_APP}
    networks:
      - ${NETWORK}
    depends_on:
      - ${DEPENDS}
    environment:
      MONGODB_HOST: mongodb-rp
      MONGODB_USERNAME: mguser
      MONGODB_PASSWORD: mgpwd123
      MONGODB_PORT: ${MGDB_PORT}
      MONGODB_DB: ${MGDB_DB}
```

Executando a imagem criada:

```
docker run --name conversaotemperatura -it -d -p 8080:8080 andersontarj/conversaotemperatura:v1
```

Para verificar o container em execução, execute o comando:

```
docker container ls
```

Terá uma saída igual a abaixo:

![Diagrama](./imgs/contlist.png)

Após a execução da imagem **andersontarj/conversaotemperatura** abra o navegador de sua preferencia e digite <http://localhost:8080>

Se tudo estiver correto você vera no navegador a tela igual a que está abaixo:

![Diagrama](./imgs/convtemp.png)
