# MongoDB com Docker Compose

Este material mostra como subir o MongoDB com Docker Compose, acessar o banco pelo `mongosh` e importar os dados do arquivo `Unifaat_aula.json`.

## O que está incluído

- MongoDB em container Docker
- Volume persistente para os dados
- Usuário root configurado com:
  - usuário: `admin`
  - senha: `admin123`
- Arquivo de dados para importação: `Unifaat_aula.json`

## Como subir o ambiente

No diretório `Helps/MongoDB`, execute:

```bash
docker-compose up -d
```

Para verificar se o container está rodando:

```bash
docker-compose ps
docker-compose logs -f
```

Para parar o ambiente:

```bash
docker-compose down
```

Para parar e remover também o volume de dados:

```bash
docker-compose down -v
```

## Como acessar o MongoDB

Você pode se conectar com o `mongosh` por dentro do container:

```bash
docker exec -it mongo_db mongosh -u admin123 -p admin123 --authenticationDatabase admin
```

Ou usando a string de conexão:

```bash
mongodb://admin123:admin123@localhost:27017/?authSource=admin
```

## Comandos principais no MongoDB

Depois de entrar no `mongosh`, os comandos mais usados são:

```javascript
show dbs
use auladb
show collections

db.pessoas.find()
db.pessoas.find().pretty()
db.pessoas.findOne()

db.pessoas.insertOne({
  nome: "Carlos",
  idade: 30,
  cidade: "Campinas",
  profissao: "Desenvolvedor"
})

db.pessoas.insertMany([
  { nome: "Maria", idade: 25, cidade: "São Paulo", profissao: "Designer" },
  { nome: "João", idade: 32, cidade: "Jundiaí", profissao: "Analista" }
])

db.pessoas.updateOne(
  { nome: "Carlos" },
  { $set: { cidade: "Atibaia" } }
)

db.pessoas.deleteOne({ nome: "Carlos" })
db.pessoas.countDocuments()
db.pessoas.drop()
```

## Importando o arquivo `Unifaat_aula.json`

Para importar o arquivo `.json` para a coleção `pessoas` do banco `auladb`, execute:

```bash
docker exec -it mongo-test mongoimport \
  -u admin \
  -p admin123 \
  --authenticationDatabase admin \
  --db auladb \
  --collection pessoas \
  --file /tmp/Unifaat_aula.json \
  --jsonArray
```

Como o comando acima lê o arquivo de dentro do container, copie o arquivo antes:

```bash
docker cp Unifaat_aula.json mongo_db:/tmp/Unifaat_aula.json
```

Fluxo completo:

```bash
docker-compose up -d
docker cp Unifaat_aula.json mongo_db:/tmp/Unifaat_aula.json
docker exec -it mongo_db mongoimport \
  -u admin123 \
  -p admin123 \
  --authenticationDatabase admin \
  --db auladb \
  --collection pessoas \
  --file /tmp/Unifaat_aula.json \
  --jsonArray
```

## Conferindo os dados importados

Entre no `mongosh` e rode:

```javascript
use auladb
show collections
db.pessoas.find().pretty()
db.pessoas.countDocuments()
```

## Requisitos

- Docker
- Docker Compose

Material preparado para uso local em aula e testes.
