# Dispositivos de Entrada e Saída
> Representam recursos ou mecanismos que possibilitam a comunicação entre o sistema e um ambiente externo.

Esses dispositivos de entrada e saída podem ser um hardware (como teclado, mouse, impressora) ou um sofware (API que permite manipulação de arquivos). No contexto de Flutter, nosso dispositivo de entrada e saída será uma API que fornece os métodos necessários para lidar com a manipulação de arquivos. Para importarmos a API precisamos utilizar `import dart:io;` , que nos possibilitará trababalhar com a classe *File*.

## Características Gerais das Operações de Entrada e Saída

* **Assincronidade:** As operações de entrada e saída precisam ser acessadas fora do escopo da aplicação, portanto, são operações assíncronas. Sendo assim, essas operações retornam um Future, possibilitando que a aplicação continue enquanto aguarda a resposta da operação.
* **Classe *File***: Representa o arquivo em questão. A classe *File* aceita um path como parâmetro. Após passado o path, a classe *File* referencia o arquivo contido no endereço que foi fornecido como parâmetro. Os métodos principais de *File* são:
	* *writeAsString*: Grava um texto no arquivo.
	* *readAsString:* Lê texto do arquivo.
	* *writeAsByte* e *readAsByte*: Trabalham com dados binários

**OBS:** A classe *File* pode trabalhar com diversos tipos de dados, como Int, Bool, Listas, etc. Para isso, ao invés de utilizar *writeAsString*, utiliza-se *writeAsInt* ou *writeAsBool*. 

É necessário levar em consideração que os métodos de escrita e leitura precisam acessar um arquivo externo a aplicação, portanto, precisam ser métodos assíncronos. Por exemplo, considere um método para salvar dados em um arquivo:
```
void _save() async{
	final file = File('/caminho/do/patch.txt');
	await file.writeAsString('Hello, Flutter!');
}
```

## Verificando a existência de um arquivo

Quando passamos um path para o *File*, o arquivo passado não é criado neste momento. O que ocorre é que *File* possui uma referência para o arquivo em questão.Somente quando utilizamos o método *writeAsString* que o arquivo é criado fisicamente. Quando utilizamos *writeAsString* em um arquivo já existente, esse arquivo será sobrescrito pelas informações novas contidas em *writeAsString*, portanto é necessário tomar cuidado ao utilizarmos os métodos de escrita. 

Podemos evitar a perda acidental de dados e oferecer um controle maior ao usuário sobre o que acontece no arquivo através de uma verificação de existência. Esse processo já é feito implicitamente ao utilizarmos o método *writeAsString*, porém é uma boa prática torná-lo explicito, pois permite um controle maior sobre o arquivo.  Por exemplo, um método de checagem de exitência de arquivo é mostrado abaixo:
```
Future<void> _checkFile(File file) async{
	if(await file.exists){
		print("Arquivo já existe!");
	}else{
		await file.create();
		print("Arquivo criado!");
	}
}
```

Note que utilizamos os métodos *create()* e *exists()*. Esses são outros métodos fornecidos pela classe *File*, que fornece informações a respeito do arquivo em si, bem como a possibilidade de criar o arquivo fisicamente de maneira manual. 

**OBS:**  Podemos utilizar o atributo `mode: FileMode.append` para evitar que o arquivo seja sobrescrito. Esse atributo fará com que o conteúdo seja adicionado ao arquivo original. Por exemplo: `await file.writeAsString('Novo Conteúdo', mode: FileMode.append)`

## Manipulação de Dados com JSON

Para armazenarmos dados mais complexos, como listas e maps, o formato JSON é amplamente utilizado. Possuindo uma estrutura chave:valor, o formato JSON permite um padrão de dados que facilita a troca entre diverentes sistemas. No contexto Flutter, temos o pacote dart:convert, que converte Listas e Maps em arquivos JSON. Segue abaixo um exemplo de como salvar e ler dados em JSON:
```
import 'dart:convert';

void _saveTask(List<Map<String,dynamic>> tasks) async{
	final file = File('tasks.json');
	final jsonString = jsonEncode(tasks);
	await file.writeAsString(jsonString);
}

Future<List<Map<String,dynamic>> _loadTask() async{
	final file = File('tasks.json');
	final jsonString = await file.readAsString();
	return List<Map<String,dynamic>>.from(jsonDecode(jsonString));
}
```

Os métodos acima permitem a conversão de um JSON para um objeto Dart através de *jsonDecode*, e um objeto Dart para um JSON, através de *jsonEncode*. 

## Leitura e Escrita de Arquivos Binários

Podemos ter a situação de precisar ler ou gravar arquivos tipo imagens, vídeos, entre outros. Para isso é necessário utilizar arquivos binários tanto para leitura quanto para gravação. E para falar em arquivos binários é necessário conhecer a biblioteca *Uint8List*. Essa biblioteca fornece métodos e maneiras de lidar de forma mais eficiente com dados binários. Portanto, os métodos *writeAsByte* e *readAsByte* aceitam o formato *Uint8List*. Segue abaixo um exemplo completo da criação, escrita e leitura de arquivos binários:
```
void saveBinaryData(Uint8List data) async{
	final file = File('data.bin');
	await file.writeAsByte(data);
}

Future<Uint8List> readBinaryData() async{
	final file = File('data.bin');
	return file.readAsByte();
}

//Criando um Uint8List
Uint8List data = Uint8List.fromList([0,1,2,255]);

//Criando um Uint8List a partir de uma imagem
Uint8List dataImage = await File('data.bin').readAsByte();
```

## Gerenciamento de Erros

Uma outra boa prática é lidar com possíveis erros que podem ocorrer quando o acesso ao arquivo é feito. Erros como falta de espaço, permissões negadas e caminho inexistente são possíveis, e é necessário tratá-los. Para isso, um bloco try-catch pode ser utilizado, fazendo com que o usuário tenha maiores informações sobre possíveis erros. Segue abaixo um exemplo de um método utilizando try-catch para tratamento de erros:
```
void _saveFile() async{
	try{
		final file = File('path/do/arquivo.txt');
		await file.writeAsString("Adicionando conteúdo!");
	}catch(e){
		print("Erro ao acessar arquivo:$e");
	}
}
```

## Resumo

A biblioteca File é essencial para lidarmos com arquivos. Através dela temos maneiras fáceis e eficientes de acessar arquivos externos. Os métodos retornados por File são assíncronos, uma vez que podem demorar para serem retornados, tornando necessário a utilização de async/await nos métodos de leitura e escrita. Algumas boas práticas podem ser feitas em relação a manipulação de arquivos, como por exemplo a criação explícita de um arquivo, que permite que o usuário tenha um controle maior sobre os arquivos, e o tratamento de exceções. A criação de arquivos pode ter diversos erros diferentes, como caminho inexistente, falta de espaço na memória e outros. Por isso o tratamento de exceções é indicado. Por fim, vimos como lidar com arquivos JSON e arquivos binários.
