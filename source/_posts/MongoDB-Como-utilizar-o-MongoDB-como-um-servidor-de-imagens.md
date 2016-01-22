title: 'MongoDB: Como utilizar o MongoDB como um servidor de imagens'
date: 2016-01-22 06:01:15
tags:
	- mongodb
	- images
	- gridfs
	- store
	- specification
---

Primeiramente nós devemos saber que essa nem sempre pode ser a melhor alternativa, ainda podemos utilizar o serviço S3 da Amazon, este guia serve apenas como uma alternativa interessante a se pensar.

Neste exemplo estaremos utilizando:
<!--more-->
 - Express 4.x
 - NodeJS 5.x

## Armazenando a imagem no banco

No exemplo abaixo, nós temos um serviço que recebe a imagem em `base64`, transforma em binário e salva no banco:

```js
var GridStore = require('mongodb').GridStore;
var randomstring = require('randomstring').generate;

// Expressão regular que será utilizada para remover
// o data:image/XXX que não faz de fato parte do código
// base64
var BASE64_REGEX = /^data:image\/png;base64,|^data:image\/jpeg;base64,|^data:image\/jpg;base64,|^data:image\/bmp;base64,/;

var FileManager = {
	nextFileId: function() {
		return `${randomstring(4)}_${randomstring(8)}_${randomstring(6)}`;
	},

	upload: function(data, encoding, fileId) {
		fileId = fileId || this.nextFileId();

		var buf = new Buffer(data, encoding),
				gridStore = new GridStore(db, fileId, 'w');

		return gridStore.open().then(function(gridStore) {
			return gridStore.write(buf);
		})
		.then(function() {
			return gridStore.close();
		})
		.then(function() {
			return fileId;
		});
	},

	uploadImage: function(data) {
		return this.upload(data.replace(BASE64_REGEX), 'base64');
	}
};
```

```js
var fs = require('fs');

function storeFileId (fileId) {
	return db.collection('images').insertOne({ fileId: fileId });
}

FileManager.uploadImage(fs.readFileSync('./images/angularjs-vs-react-who-is-the-winner.jpg'))
.then(storeFileId);
```

### Criando a instância do GridStore

No primeiro parâmetro nós passamos a ligação para a conexão com o banco de dados (que você receberá logo após um `require('mongodb').MongoClient.connect()`), no segundo parâmetro nós definimos o nome do arquivo e por último nós definimos o que vamos fazer nesse documento, no nosso caso, vamos ler. Portanto `w` que vem de `write` :)

### Gerando o nome do arquivo

Você pode notar na `linha 13` do exemplo acima que o nome do arquivo que vamos guardar é criado pela função anônima na propriedade `nextFileId` do `FileManager`. Ele retorna algo como:

```
c9R9_B7xgEVxQ_u7nOX9
```

É apenas para termos certeza de que nenhum arquivo fique com nomes iguais e que possamos colocar isso numa url sem que fique na cara que utilizamos MongoDB:

```
https://api.dominio.com.br/images/c9R9_B7xgEVxQ_u7nOX9.png
```

Mas nada te impede de usar um `ObjectId`, we're talking about Mongo, right? :)

### Escrevendo no arquivo

Na `linha 19` nós armazenamos os dados da imagem na instância, mas caso você não execute o `gridStore.close()` como na `linha 22`, ele não será salvo no banco e ficará apenas na instância. Então é sempre importante executar um `gridStore.close()` após o `gridStore.write()`. O parâmetro que colocamos em `write()` pode ser um `Buffer` ou uma `String`

## Lendo a imagem previamente armazenada

Para acessarmos a imagem que salvamos, vamos precisar de utilizar novamente a classe GridStore, só que dessa vez não precisaremos criar outra instância, mas definitivamente vamos precisar do id do arquivo que armazenamos, portanto é importante que você saiba que é necessário guardar a identificação do arquivo para que você possa acessá-la futuramente.

```js
var Q = require('q'),
		GridStore = require('mongodb').GridStore;

FileManager.get = function(id) {
	return GridStore.exists(db, id).then(function(result) {
		if(result) {
			return GridStore.read(db, id);
		}

		return Q.reject(new Error('File does not exists'));
	});
};
```

```js
class AlbumController {
	getUserAlbum(req) {
		return db.collection('users').findOne({
			_id: ObjectId(req.params.userId)
		})
		.then(function(user) {
			var userId = user._id.toString();

			return db.collection('albums').findOne({ userId: userId });
		})
		.then(function(album) {
			return albums && album.images && Q.all(album.images.forEach(function(image) {
				return FileManager.get(image.fileId);
			}));
		});
	}
}
```

## Criando um endpoint

Eu acredito que essa seja a parte mais simples de tudo: Criar um endpoint em que você possa requisitar a sua imagem apartir do seu site.

```js
var EXT_REMOVE_REGEX = /\.([A-z]+)$/;

app.get('images/:imageId', function(req, res) {
	var ext, filename = req.params.id;

	// Vamos remover a extensão do arquivo, caso você tenha
	// digitado algo como "Z97d_KLEJAcVI_kII2tZ.png" ou
	// "Z97d_KLEJAcVI_kII2tZ.jpg"
	filename = filename.replace(EXT_REMOVE_REGEX, function(raw, $ext, index, filename) {
		ext = $ext;

		return '';
	});

	// Vamos preencher esta variável para futuramente podermos
	// reutilizar quando formos escrever o cabeçalho da resposta.
	ext = ext || 'png';

	return FileManager.get(filename).then(function(buf) {
		// Escrevendo no cabeçalho com a nova imagem e o tamanho
		// do buffer.
		res.writeHead(200, {
			'Content-Type': 'image/' + ext,
			'Content-Length': buffer.length
		});
	

		// Respondendo a requisição com os dados da imagem, que
		// deverá ser carregada normalmente pelo seu navegador.
		res.end(buf, 'binary');
	});
});
```

Espero que tenham gostado, deixem suas opiniões e dúvidas abaixo. Até a próxima.