title: 'MongoDB: Pesquisa em raios de distância com coordenadas usando o $geoWithin e $centerSphere'
date: 2015-09-06 23:46:05
tags: mongodb node
---

Recentemente eu me deparei com o seguinte problema:

Eu precisava verificar se uma lista de coordenadas estava ou não dentro de um dado raio de distância. A princípio eu pensei em utilizar a API do Google Maps, porém a única funcionalidade que faz esse tipo de pesquisa está disponível apenas na versão JavaScript, e a minha API estava escrita em Node.js e ficaria um pouco complicado de utilizar mais da cota para fazer uma pesquisa geográfica... Até que eu descobri o `$centerSphere` e o `$geoWithin` do MongoDB, e foi aí que eu vi que realmente MongoDB é o banco de dados do futuro... Hahaha.
<!--more-->

### Requisitos
- MongoDB 3.0

Primeiramente, como diz na documentação do MongoDB, a respeito do [$centerSphere](http://docs.mongodb.org/manual/reference/operator/query/centerSphere/), é necessário saber que as coordenadas de latitude e longitude devem estar dentro de um `Array` para que o MongoDB possa fazer a pesquisa, mais ou menos da seguinte forma:

```js
{
	"name": "Victor Queiroz",
	"address": {
		"coords": [
			7.0202512,
			4.8499534
		]
	}
}
```

A consulta deve ser escrita da seguinte forma:
```js
var coords = [
	7.0398232,
	4.8492161
];

var miles = 1.8;

db.users.find({
	'address.coords': {
		$geoWithin: {
			$centerSphere: [
				coords,
				miles / 3963.2
			]
		}
	}
})
```

O MongoDB vai definir um círculo de X milhas, sendo `coords` o centro do raio, e fará uma pesquisa dentro de `address.coords` procurando por coordenadas que estejam presentes dentro desse círculo.

No caso eu defini um raio de 1.8 milhas, que equivalem a 3km, novamente: **Ele irá me retornar todos os documentos que estiverem dentro deste raio**.

**Lembre-se que a longitude deve estar ANTES da latitude dentro das listas.**

Abaixo estão algumas descrições básicas sobre os operadores que utilizamos nessa consulta:

### $centerSphere
```js
{
	<location field>: {
		$geoWithin: { $centerSphere: [ [ <x>, <y> ], <radius> ] }
	}
}
```

Define um círculo para qualquer consulta geoespacial que utilize [geometria esférica](https://pt.wikipedia.org/wiki/Geometria_esf%C3%A9rica).

### $geoWithin
```js
{
	<location field>: {
		$geoWithin: {
			$geometry: {
				type: <"Polygon" or "MultiPolygon"> ,
				coordinates: [ <coordinates> ]
			}
		}
	}
}
```

Seleciona o documento com dados geoespaciais que existem inteiramente em um formato especificado. Ao determinar a inclusão, o MongoDB considera a fronteira de uma forma como se fosse parte da forma, sujeito à precisão dos números de ponto flutuante.

Para mais informações, acesse a [documentação](https://docs.mongodb.org/manual/reference/operator/query/geoWithin/) oficial do MongoDB e leia a respeito.

Espero que o artigo ajude a muitos da forma como me ajudou. Abraços.