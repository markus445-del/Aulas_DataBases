1: Criar o banco de dados loja_virtual
use loja_virtual
Criar a coleção produtos

No MongoDB, a coleção é criada automaticamente ao inserir o primeiro documento.
Mas se quiser criar explicitamente:
db.createCollection("produtos")

Inserir um produto com insertOne()
db.produtos.insertOne({
  nome: "Smartphone Samsung Galaxy S23",
  categoria: "Eletrônicos",
  preco: 3500,
  estoque: 15
})

Agora inserindo produtos de categorias diferentes:

db.produtos.insertMany([
  {
    nome: "Camiseta Básica",
    categoria: "Roupas",
    preco: 50,
    estoque: 100
  },
  {
    nome: "Notebook Dell Inspiron",
    categoria: "Eletrônicos",
    preco: 4500,
    estoque: 10
  }
])

Para verificar se os dados foram inseridos:

db.produtos.find()

2: Listar todos os produtos
db.produtos.find()

Buscar produtos com preço maior que 100
db.produtos.find({
  preco: { $gt: 100 }
})

Listar produtos da categoria "Eletronicos"
db.produtos.find({
  categoria: "Eletronicos"
})

Retornar apenas nome e preco
db.produtos.find(
  {},
  { nome: 1, preco: 1, _id: 0 }
)

3: Atualizar o preço de um produto específico ($set)
db.produtos.updateOne(
  { nome: "Smartphone Samsung Galaxy S23" },
  { $set: { preco: 3200 } }
)

Adicionar o campo estoque para todos os produtos (updateMany)
db.produtos.updateMany(
  {},
  { $set: { estoque: 50 } }
)

Marcar produtos da categoria "Roupas" em promoção
db.produtos.updateMany(
  { categoria: "Roupas" },
  { $set: { promocao: true } }
)

4: Remover um produto específico (deleteOne())
db.produtos.deleteOne({
  nome: "Camiseta Básica"
})

Remover todos os produtos de uma categoria (deleteMany())
db.produtos.deleteMany({
  categoria: "Roupas"
})

db.produtos.deleteMany({})

 Isso apaga TODOS os documentos da coleção 