# Future e FutureBuilder

> Em dart, um Future é uma maneira de lidar com operações assíncronas. 

É uma abstração usada para lidar com operações assíncronas, permitindo com que a gente execute tarefas em segundo plano, sem bloquear a thread principal. Geralmente, em requisições http utilizamos a classe Future para fazer o acesso ao endpoint do servidor. 

Future está diretamente ligado ao FutureBuilder, onde future define a lógica e a maneira como os dados seráo retornados, e o FutureBuilder contrói a interface baseada nesses dados dinamicamente, ou seja, sempre que o Future é resolvido a interface será renderizada para representar essas modificações. FutureBuilder reconstrói sua interface automaticamente, sempre quando um Future é resolvido, sem que haja a necessidade de utilizar um setState manualmente. 

## Utilizando uma classe Future para lidar com requisições http

O código abaixo acessa uma API responsável por buscar CEPs digitados pelo usuário. Essa classe deve ser assíncrona, pois o acesso ao método desejado é feito fora da aplicação principal. Segue abaixo como o código é estruturado:

```
Future<Map> getCepFuture() async{

    if(_cepController.text.isEmpty){
      return{
	      "logradouro":"",
	      "bairro":"",
	      "localidade":"",
	      "uf":"",
	      "complemento":""
      };
    }

    String url = 'https://viacep.com.br/ws/${_cepController.text}/json/'; 

    http.Response response = await http.get(Uri.parse(url));
    
    Map<String,dynamic> cepData = jsonDecode(response.body);

    return cepData;  
  }
```

Considerações sobre o código:
* O método getCepFuture() é assíncrono e retorna um objeto do tipo *Future< Map>*, isso faz com que se possa utilizar os dados retornados após a resolução do Future.
* O primeiro `if(_cepController.text.isEmpty)` verifica se o texto contido no controller é nulo. Caso positivo, o Future retornará um objeto JSON com os valores em branco.
* A url guarda o endereço do endpoint referente ao cep desejado (que será passado via TextField).
* Uma variável chamada response foi criada para receber os dados. O tipo da variável é http.Response, que representa um objeto capaz de receber dados de requisições http. Esse acesso necessita de um await, pois é feito fora da aplicação.
* O método http.get aceita como parâmetro uma Uri, então temos que transformar nossa url numa Uri através do método parse. O retorno de http.get é dado no formato JSON, portanto, precisamos mapear esse formato em uma variável do tipo chave:valor (map).
* A variável cepData é do tipo Map<String,dynamic>, e é responsável por receber os valores do arquivo JSON em algo que possa ser lido pelo compilador. O mapeamento é feito através do método jsonDecode(response.body).
* Por último retornamos o objeto Future, chamado cepData. cepData contém todos os dados retornados pela API, e nós escolhemos quais dados queremos exibir.

## Exibindo os dados retornados pelo Future através de FutureBuilder

Agora que temos os dados em um objeto do tipo Future, podemos exibir esses dados da maneira desejada. Isso é feito de forma dinâmica através do Widget FutureBuilder. Este widget possui dois atributos essenciais, *builder* e *future*, onde o primeiro é responsável pela construção da interface em si, e o segundo é responsável por obter o objeto de interesse. 

Segue abaixo o código referente ao FutureBuilder:
```
FutureBuilder(
  future: getCepFuture(),
  builder: (context, snapshot){
	  String result = ""
	  
	  if(snapshot.hasData){
		  var cepData = snapshot.data!;
          result = cepData['logradouro'];
          rua = cepData['logradouro']!;
          bairro = cepData['bairro']!;
	      cidade = cepData['localidade']!;
          estado = cepData['uf']!;
          complemento = cepData['complemento']!;

          return Column(
			  children:[
	              Text("Rua:$rua"),
                  Text("Bairro:$bairro"),
                  Text("Cidade:$cidade"),
                  Text("Estado:$estado"),
                  Text("Complemento:$complemento"
                  ]
               );
        }
      else if(snapshot.hasError){
	      result = snapshot.error.toString();
	      }
      else{
	      return CircularProgressIndicator();
	     }     
	     
	  return Text("Resultado: $result");
}),
```

Considerações sobre o código:
* Precisamos de um objeto do tipo Future para preencher o atributo future. Para isso, utilizamos nosso método *getCepFuture()*, que retorna o future que conterá os dados desejados. 
* Em *builder* nós adicionamos a tela renderizada. Para isso, passamos como parâmetro snapshot, que receberá os dados do Future. 
* Em `if(snapshot.hasData)` verifica-se se o objeto Future possui algum dado. Se sim, atribui-se uma variável responsável por recber esses dados. No caso acima utilizamos `var cepData = snapshot.data!`.
* Após atribuirmos os dados a cepData, precisamos passar cada um desses dados para as variáveis referentes aos atributos rua, bairro, cidade, estado e complemento. 
* Por último, retornamos um widget responsável por exibir nossos dados na interface. Esse widget no caso acima foi o Column, onde cada elemento do children continha o texto referente aos dados retornados.
* Em `else if(snapshot.hasError)` nós exibimos uma mensagem para o usuário informando o erro.
* E por último, caso o Future ainda esteja sendo resolvido, um circulo indicando o progresso será exibido. 

## Considerações

O Future é utilizado para quando desejamos realizar requisições assíncronas, como em APIs que utilizam os principios de RESTful. Quando utilizamos uma classe *Future*, precisamos utilizar um widget *FutureBuilder* em conjunto, permitindo com que a gente consiga construir uma interface de maneira dinâmica. A utilização dessa classe torna o código mais simplificado, diminuindo a redundância e tornando o código mais legível. 
