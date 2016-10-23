# mongodbintro-webschool-pupilo

## O que é o MongoDB ?

MongoDB é um Banco de Dados noSQL (Not Only SQL), o que significa que ele é um banco não relacional, qualquer banco que não seja Relacional será um banco NoSQL. Todos seus recursos, características e funcionalidades foram desenvolvidas pensando um Banco de Dados Orientado a Documentos (Document Database). Para trabalhar com o MongoDb iremos utilizar como linguagem de acesso direto, o JavaScript, e a modelagem de como os dados ficarão, nós utilizamos JSON (JavaScript Object Notation).

Exemplo:

```js
{
	"_id": "462606",
	"nome": "Sistema On",
	"endereco": {
			"nomeRua": "Avenida do Contorno",
			"numero": "3498",
			"complemento": "Apartamento 302",
			"referencia": "Esquina com Avenida Assis Chateaubriand",
			"bairro": "Floresta",
			"cidade": "Belo Horizonte",
			"Estado": "Minas Gerais",
			"Pais": "Brasil"
	},
	"hobbies": [ "games", "bike", "artesão cervejeiro", "rock n roll", "tea"  ]
}

```

Vantagem muito importante deste design é que equipes de desenvolvimento podem muito bem projetar modelos de dados para suportar o acesso de dados comuns.

Como por exemplo, uma equipe desenvolvendo um site de notícias podem estruturar seu modelo de dados, para que a página mais vista necessite apenas de uma única consulta (query) do banco de dados, e pode ser feito de uma maneira elegante e é suportado pelo MongoDB.

Comparando com uma estrutura relacional, exigindo alguns joins entre as tabelas, ou uma maneira desnecessária de armazenar valores múltiplos em um único campo de uma tabela.

Como o MongoDB não baseia em junção de dados vindos de múltiplas tabelas, fica mais simples de distribuir/sharding os dados em vários servidores, com isso você terá uma facilidade maior para escalar horizontalmente, não precisando utilizar máquinas muito poderosas, mas sim diversas pequenas máquinas para a criação de um cluster.

MongoDB escala horizontalmente através de Sharding e Replicas de uma forma simples e transparente, afastando esse gerenciamento da lógica da aplicação para que desenvolvedores possam construir sistemas altamente disponíveis sem muito trabalho.

Se é em um único dispositivo ou em centenas, isso não faz diferença do ponto de vista da aplicação, porém joins e operações em várias tabelas adicionam uma carga de trabalho computacional que pode sobrecarregar o servidor, em um sistema relacional, sua melhor opção é scaling up (escalar pra cima / escalabilidade vertical) e com isso aumentar cada vez mais os custos para que os dados estejam disponíveis de um único servidor.

MongoDB funciona sem esquemas (schemas), logo todo o trabalho de gerenciar a integridade desses dados fica a cargo da aplicação/sistema. Como ele não possui Schemas você poderá inserir qualquer tipo de valor em qualquer collection, entretanto isso logicamente é ruim pois perdemos a integridade dos dados. Para solucionar esse problema utilizamos projetos como o Mongoose que nos provê toda essa funcionalidade além de muitas outras.
O bom dele não possuir Schema é que você poderá modificar esse Schema, via Mongoose, e isso não irá impactar no resto dos dados.
Para deixar bem claro: o Schema é onde definimos todos os campos desejados e seus tipos, muito parecido com a etapa de CREATE TABLE dos bancos relacionais, com o diferencial que toda essa configuração fica no nosso código.

Em resumo MongoDB permite que os desenvolvedores projetam modelos de dados que eficientemente suportam padrões de acesso de dados comum, também é projetado para suportar práticas de engenharia de software ágil e atender a escalabilidade e necessidades de aplicações modernas de desempenho.

### CRIANDO DOCS

No MongoDB, para criar um documento, podemos inserir 	no nosso banco dentro da nossa collection (colecão). Podemos utilizar o comando use nomeBanco para criar o Banco, e o método db.nomeCollection.insertOne() para adicionar documentos na nossa colecão. Não precise criar a colecão, pois se não existir, e ao inserir documentos, o Mongo automaticamente cria e insere em nosso banco.


```js
<Terminal>
use mongonode
mongonode> db.users.insertOne({ "nome": "Alex", "idade": 23, "casado": false  });
{
  "acknowledged": true,
  "insertedId": ObjectId("580940d0cd5972fe6179f714")
}
</Terminal>

```

Para buscar os documentos, usamos o método `db.nomeCollection.find()`


```js
<Terminal>
mongonode> db.users.find()
{
  "_id": ObjectId("580940afcd5972fe6179f713"),
  "nome": "Alex",
  "idade": 23,
  "casado": false
}
</Terminal>

O MongoDB criar automaticamente o id que é _id ("_id": ObjectId("123etc")) para cada documento inserido. Porém nos permite passar o nosso próprio _id.
<Terminal>
mongonode> db.users.insertOne({ "_id": "un0001", "nome": "Alex", "idade": 23, "casado": false  });
{
  "acknowledged": true,
  "insertedId": "un0001"
}

mongonode> db.users.find()
{
  "_id": "un0001",
  "nome": "Alex",
  "idade": 23,
  "casado": false
}
</Terminal>

```




Para excluir uma collection , utilizamos o método db.nomeCollction.drop()
<Terminal>
mongonode> db.users.drop()
true
mongonode> db.users.find()
Fetched 0 record(s) in 1ms
</Terminal>

Para adicionar vários documentos de uma vez na collection, O MongoDB possui um método db.nomeCollection.insertMany() , em que ele espera um array:


```js
<Terminal>
mongonode> db.users.insertMany(
    [
        {
    "nome" : "Bruno",
    "idade" : 22,
    "casado" : false,
        },
        {
    "nome" : "Camila",
    "idade" : 25,
    "casado" : false,
        },
        {
    "nome" : "Daise",
    "idade" : 27,
    "casado" : true,
        },
        {
    "nome" : "Amanda",
    "idade" : 20,
    "casado" : false,
        },
    ]
);
{
  "acknowledged": true,
  "insertedIds": [
    ObjectId("58094719cd5972fe6179f719"),
    ObjectId("58094719cd5972fe6179f71a"),
    ObjectId("58094719cd5972fe6179f71b"),
    ObjectId("58094719cd5972fe6179f71c")
  ]
} 

mongonode> db.users.find()
{
  "_id": ObjectId("58094719cd5972fe6179f719"),
  "nome": "Bruno",
  "idade": 22,
  "casado": false
}
{
  "_id": ObjectId("58094719cd5972fe6179f71a"),
  "nome": "Camila",
  "idade": 25,
  "casado": false
}
{
  "_id": ObjectId("58094719cd5972fe6179f71b"),
  "nome": "Daise",
  "idade": 27,
  "casado": true
}
{
  "_id": ObjectId("58094719cd5972fe6179f71c"),
  "nome": "Amanda",
  "idade": 20,
  "casado": false
}
</Terminal>

```

Como o MongoDB permite passarmos o nosso próprio _id , caso o _id esteja duplicado ao inserir na collection , quando o MongoDB encontra o _id duplicado, ele simplesmente para de inserir documentos na colecão, e nos passa um erro, porém insere todos os documentos anteriores (antes de passar o erro). 

```js
<Terminal>
db.users.insertMany(
    [
        {
      "_id": "un0001",
	    "nome" : "Bruno",
	    "idade" : 22,
	    "casado" : false,
        },
        {
      "_id": "un0002",
	    "nome" : "Camila",
	    "idade" : 25,
	    "casado" : false,
        },
        {
      "_id": "un0003",
	    "nome" : "Daise",
	    "idade" : 27,
	    "casado" : true,
        },
        {
      "_id": "un0003",
	    "nome" : "Amanda",
	    "idade" : 20,
	    "casado" : false,
        },
    ]
);

```

E QUERY    [thread1] BulkWriteError: write error at item 3 in bulk operation :

```js
BulkWriteError({
  "writeErrors": [
    {
      "index": 3,
      "code": 11000,
      "errmsg": "E11000 duplicate key error collection: mongonode.users index: _id_ dup key: { : \"un0003\" }",
      "op": {
        "_id": "un0003",
        "nome": "Amanda",
        "idade": 20,
        "casado": false
      }
    }
  ],
"writeConcernErrors": [ ],
  "nInserted": 3,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})
</Terminal>

```

Repare nessa parte do erro:
"writeErrors": [
    {
      "index": 3,
      "code": 11000,
      "errmsg": "E11000 duplicate key error collection: mongonode.users index: _id_ dup key: { : \"un0003\" }",
      "op": {
        "_id": "un0003",
        "nome": "Amanda",
        "idade": 20,
        "casado": false
      }
    }
  ]

O MongoDB está reclamando do index 3 ao ser inserido, e sua mensagem de erro (errmsg) é chave duplicada (duplicate key) na collection dentro do banco, e logo em seguida, passando o documento que não pôde ser inserido.




Repare nessa outra parte do erro:
"nInserted": 3

Isso significa que o MongoDB inseriu 3 documentos, no qual, foram os 3 primeiros, e ao encontrar o erro, ele parou de inserir e nos enviou o erro.
<Terminal>
mongonode> db.users.find()
{
  "_id": "un0001",
  "nome": "Bruno",
  "idade": 22,
  "casado": false
}
{
  "_id": "un0002",
  "nome": "Camila",
  "idade": 25,
  "casado": false
}
{
  "_id": "un0003",
  "nome": "Daise",
  "idade": 27,
  "casado": true
}
</Terminal>

MongoDB possui um segundo argumento (parâmetro) chamado ordered que possamos passar ao inserir os documento, o que ele faz é, ao encontrar algum erro ele ainda vai tentar inserir os documentos restantes, e ao finalizar seu processo, ae sim que, ele nos envia o erro. O seu valor por padrão é true, e para que o MongoDB continue realizando as inserções mesmo ao econtrar um erro, devemos passar o seu valor como false.

<Terminal>
db.users.insertMany(
    [
        {
      "_id": "un0001",
	    "nome" : "Bruno",
	    "idade" : 22,
	    "casado" : false,
        },
        {
      "_id": "un0002",
	    "nome" : "Camila",
	    "idade" : 25,
	    "casado" : false,
        },
        {
      "_id": "un0003",
	    "nome" : "Daise",
	    "idade" : 27,
	    "casado" : true,
        },
        {
      "_id": "un0003",
	    "nome" : "Amanda",
	    "idade" : 20,
	    "casado" : false,
        },
        {
      "_id": "un0004",
	    "nome" : "Alberto",
	    "idade" : 33,
	    "casado" : true,
        },
    ],
    
    {
      "ordered": false
    }
);

E QUERY    [thread1] BulkWriteError: write error at item 3 in bulk operation :
BulkWriteError({
  "writeErrors": [
    {
      "index": 3,
      "code": 11000,
      "errmsg": "E11000 duplicate key error collection: mongonode.users index: _id_ dup key: { : \"un0003\" }",
      "op": {
        "_id": "un0003",
        "nome": "Amanda",
        "idade": 20,
        "casado": false
      }
    }
  ],
  "writeConcernErrors": [ ],
  "nInserted": 4,
  "nUpserted": 0,
  "nMatched": 0,
  "nModified": 0,
  "nRemoved": 0,
  "upserted": [ ]
})
</Terminal>

Olhe novamente nessa parte do erro:
"writeErrors": [
    {
      "index": 3,
      "code": 11000,
      "errmsg": "E11000 duplicate key error collection: mongonode.users index: _id_ dup key: { : \"un0003\" }",
      "op": {
        "_id": "un0003",
        "nome": "Amanda",
        "idade": 20,
        "casado": false
      }
    }
  ]

Nada mudou comparando com o erro anterior, MongoDB continua reclamando do documento do index 3 ao ser inserido, erro de chave duplicada e novamente passando o documento que não pôde ser inserido.

Mas repare nessa outra parte do erro:
"nInserted": 4

MongoDB ‘ignorou’ o documento com chave duplicada, inseriu os documentos restantes e logo depois, passou o erro.

<Terminal>
mongonode> db.users.find()
{
  "_id": "un0001",
  "nome": "Bruno",
  "idade": 22,
  "casado": false
}
{
  "_id": "un0002",
  "nome": "Camila",
  "idade": 25,
  "casado": false
}
{
  "_id": "un0003",
  "nome": "Daise",
  "idade": 27,
  "casado": true
}
{
  "_id": "un0004",
  "nome": "Alberto",
  "idade": 33,
  "casado": true
}
</Terminal>


Por padrão, o Object Id é criado automaticamente pelo MongoDB caso não forneça um. Todas as collections possui um index primário único em seu campo _id, isso permite eficientimente buscar documentos pelo seu próprio _id. Como dito antes, por padrão, MongoDB cria valores para o campo _id do tipo Object ID, que é um tipo de valor definido em spec (especificação) BSON.
E é estruturada da seguinte forma:

					                                Imagem de https://university.mongodb.com

Significa que, o banco de dados utiliza esses valores a fim de construir um ObjectId. O ObjectId são valores strings hexadecimal de 12 byte.
Os 4 primeiros valores byte (Date), são valores representando os segundos desde a era Unix (Unix epoch) – (https://pt.wikipedia.org/wiki/Era_Unix).
Os próximos 3 valores byte (Mac Addr), vem do identificador da máquina. São do mac address da máquina em que o MongoDB está em execução.
Logo após tem os 2 valores byte que contém o ProcessID (PID).
E os últimos 3 valores byte são um contador (Counter), garantindo que todos os ObjetctId são únicos.


LENDO DOCS
No MongoDB podemos ler os documentos com o método find, passando como db.nomeCollection.find(), porém dessa forma, o banco retornará todos os documentos encontrados naquela collection. Porém ele nos permite passar query, assim podemos buscar documentos específicos. 
<Terminal>
mongonode> var query = {casado: false}
mongonode> db.users.find(query)
{
  "_id": ObjectId("580a6721168b997a79a58035"),
  "nome": "Bruno",
  "idade": 22,
  "casado": false
}
{
  "_id": ObjectId("580a6721168b997a79a58036"),
  "nome": "Camila",
  "idade": 25,
  "casado": false
}
{
  "_id": ObjectId("580a6721168b997a79a58038"),
  "nome": "Amanda",
  "idade": 20,
  "casado": false
}
</Terminal>


MongoDB permite contar (count) os documentos que possuímos no banco. O count retorna a contagem de documentos que corresponde a uma query do find().
<Terminal>
mongonode> var query = {casado: false}
mongonode> db.users.find(query).count()
3
</Terminal>

* Vamos trabalhar com uma collection que possui mais informações. Drop sua collection antiga e insert a nova collection (usersMongo.json).

Podemos utilizar selectores adicionais em nossa query ao buscar documentos.
<Terminal>
mongonode> var query = {casado: false, idade: 21}
mongonode> db.users.find(query)
{
  "_id": ObjectId("580b79fdb3b450c6b814a2d1"),
  "nome": "Sargent Kemp",
  "idade": 21,
  "casado": false,
  "poupanca": "R$3,003.14",
  "endereco": {
    "numero": 677,
    "rua": "Calder Place",
    "cidade": "Glidden",
    "estado": "Alaska",
    "cep": 37378
  },
  "hobbies": [
    "bike",
    "rock n roll",
    "teatro",
    "viajar"
  ]
}
{
  "_id": ObjectId("580b79fdb3b450c6b814a2d6"),
  "nome": "Robertson Roy",
  "idade": 21,
  "casado": false,
  "poupanca": "R$1,987.49",
  "endereco": {
    "numero": 805,
    "rua": "Matthews Place",
    "cidade": "Klondike",
    "estado": "Florida",
    "cep": 36350
  },
  "hobbies": [
    "livros",
    "natação",
    "teatro",
    "tecnologia"
  ]
}
</Terminal>


MongoDB permite que possamos buscar documentos embutidos (embedded documents), tais documentos em array e outras estruturas aninhadas (nested structure).

Repare na seguinte estrutura:
 "endereco": {
    "numero": Number,
    "rua": String,
    "cidade": String,
    "estado": String,
    "cep": Number
  }

Essa estrutura é uma estrutura aninhada, para que possamos fazer qualquer tipo de busca em uma estrutura como esta, utilizamos ponto (.), em outras palavras notação de ponto (dot notation).
A sintaxe da notação de ponto funciona para documentos aninhados em qualquer nível.
Temos um campo endereco, e este campo armazena um documento aninhado com os demais campos dentro do campo endereco.
Para que possamos buscar um documento com o nome de um estado específico, California por exemplo, nesta estrutura, passamos a query da seguinte forma, "endereco.estado": "California" .

<Terminal>
mongonode> var query = {"endereco.estado": "California"}
mongonode> db.users.find(query)
{
  "_id": ObjectId("580b79fdb3b450c6b814a2c4"),
  "nome": "Ramsey Silva",
  "idade": 23,
  "casado": true,
  "poupanca": "R$3,822.36",
  "endereco": {
    "numero": 847,
    "rua": "Wilson Avenue",
    "cidade": "Gambrills",
    "estado": "California",
    "cep": 7452
  },
  "hobbies": [
    "games",
    "rock n roll",
    "teatro",
    "viajar"
  ]
}
</Terminal>


CURSORS
Cursor é um ponteiro (pointer) ao conjunto de resultados de uma query. Podemos percorrer um cursor para obter resultados. 
O método find retorna um cursor. Vamos fazer um pequeno exemplo utilizando o método find e percorrer seu cursor para buscar resultados de uma query.
Primeiro criaremos uma variável passando nossa query, depois iremos atribuir nosso cursor em uma variável, logo após, iremos criar uma função passando o nome da nossa collection como parâmetro e que retorne algumas informações dos usuários, e por último, a mágica acontecendo, onde estamos percorrendo o nosso cursor para buscar resultados.
Veja mais sobre cursor na documentação do MongoDB (https://docs.mongodb.com/v3.0/core/cursors/). 

<Terminal>
mongonode> var query = {casado: false}
mongonode> var cursor = db.users.find(query)
mongonode> var fn = function(users) { return "nome: " + users.nome + ". Idade: " + users.idade + ". Hobbies: " + users.hobbies + "."; }
mongonode> cursor.map(fn)
[
  "nome: Liz Walters. Idade: 25. Hobbies: livros,natação,yoga,viajar.",
  "nome: Jennifer Nash. Idade: 24. Hobbies: bike,filmes,teatro,tecnologia.",
  "nome: Victoria Medina. Idade: 31. Hobbies: bike,rock n roll,yoga,tecnologia.",
  "nome: Cecile Doyle. Idade: 37. Hobbies: bike,rock n roll,teatro,viajar.",
  "nome: Ollie Calhoun. Idade: 22. Hobbies: bike,natação,yoga,viajar.",
  "nome: Sargent Kemp. Idade: 21. Hobbies: bike,rock n roll,teatro,viajar.",
  "nome: Case Whitney. Idade: 27. Hobbies: bike,natação,yoga,tecnologia.",
  "nome: Olive Castro. Idade: 33. Hobbies: livros,rock n roll,teatro,tecnologia.",
  "nome: Robertson Roy. Idade: 21. Hobbies: livros,natação,teatro,tecnologia.",
  "nome: Rodgers Barnett. Idade: 33. Hobbies: livros,filmes,yoga,tecnologia.",
  "nome: Spence Mcguire. Idade: 19. Hobbies: games,filmes,teatro,tecnologia.",
  "nome: Becker Wood. Idade: 25. Hobbies: games,rock n roll,yoga,viajar.",
  "nome: Courtney Holden. Idade: 18. Hobbies: livros,correr,teatro,viajar.",
  "nome: Alvarez Vasquez. Idade: 29. Hobbies: games,correr,teatro,viajar.",
  "nome: Donaldson Dillon. Idade: 19. Hobbies: bike,filmes,yoga,tecnologia."
]
</Terminal>
