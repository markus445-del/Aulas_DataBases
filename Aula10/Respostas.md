1: 1:{
  "_id": 101,
  "titulo": "Banco de Dados NoSQL",
  "categoria": "Tecnologia",
  "carga_horaria_total": 40,
  "modulos": [
    {
      "nome": "Introdução ao NoSQL",
      "carga_horaria": 5
    },
    {
      "nome": "Modelagem de Documentos",
      "carga_horaria": 10
    },
    {
      "nome": "Consultas e Índices",
      "carga_horaria": 12
    },
    {
      "nome": "Projeto Final",
      "carga_horaria": 13
    }
  ]
}

1: 2:Porque os módulos fazem parte direta do curso e normalmente são acessados junto com ele.
Ao incorporar os módulos dentro do documento:

uma única consulta retorna todas as informações;
evita múltiplas buscas no banco;
reduz operações de junção (join) ou consultas adicionais;
melhora a performance e diminui a latência de leitura.

Esse modelo é ideal quando:

os dados relacionados possuem relação forte;
os módulos dificilmente existem sem o curso;
as informações são lidas com frequência em conjunto.

2: 1: db.clientes.find({
  email: { $exists: true }
})

2: 2: db.clientes.find({
  idade: { $gte: 21 }
})

2: 3: db.clientes.find({
  cidade: "São Paulo",
  idade: { $lt: 30 }
})

3: db.vendas.aggregate([
  {
    $match: {
      status: "concluída"
    }
  },
  {
    $group: {
      _id: "$categoria",
      total_quantidade_vendida: {
        $sum: "$quantidade"
      }
    }
  }
])

