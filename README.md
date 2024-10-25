# GrupoA Educação - Full Stack Web Developer - Laravel

## Frontend em Vue.JS com Vuetify

Para acessar uma base de exemplo com o codigo implementado
nesse desafio vocês pode acessar o seguinte link:
(https://challenge-grupo-a.netlify.app/)

## Como executar o projeto 

### Com node e npm instalado na maquina

- Rodar o seguinte comando para instalar as dependencias do projeto
```console
npm install
```

- Copiar o arquivo .env.example para .env
```console
cp .env.example .env
```

- Dentro do arquivo .env apontar para o servidor das APIS
```console
VUE_APP_API_URL="Servidor"
```

- Para iniciar o projeto, rodar o seguinte comando
```console
npm run dev
```

### Com docker

- Rodar o seguinte comando para criar o arquivo .env
```console
cp .env.example .env
```

- Rodar o seguinte comando para criar a imagem do docker
```console
docker-compose build
```

- Rodar o seguinte comando para iniciar os containers Docker
```console
docker-compose up -d
```

- Rodar o seguinte comando para acessar o shell do container
```console
docker exec -it vite_docker sh
```

- Dentro do shell do container rodar o seguinte comando para instalar as dependencias
```console
npm install
```

- Rodar o seguinte comando para iniciar o projeto
```console
npm run dev
```


## Acessar o projeto

Acessar o projeto localmente pelo link: http://localhost:3000

