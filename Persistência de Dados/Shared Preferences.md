# Shared Preferences

> É uma API fornecida pelo Flutter para armazenar dados localmente no dispositivo, de forma persistente

Shared preferences permite com que o usuário consiga guardar um pequeno volume de dados no formato *chave:valor*. É especialmente útil para guardar informações sobre preferências de navegação, como definir um tema (como dark, por exemplo), informações sobre persistência de logins e outros. Como a inserção e a manipulação dos dados é feito diretamente através de uma chave, é possível que haja sobrescrita de dados caso se utilize a mesma chave, portanto, não é recomendado o uso da API Shared Preferences para guardar dados sensíveis. 

## Primeiro passo: adicionando dependências no pubspec.yaml

Para termos acesso aos métodos disponíveis em *Shared Preferences* precisamos adicioná-la nas dependências da aplicação, através do arquivo *pubspec.yaml*, conforme o exemplificado abaixo:
```
dependencies:
  shared_preferences: ^2.0.15
```

Após a instalação da dependência será possível utilizar os métodos necessários para comunicação com a API. 

## Principais métodos utilizados

O método principal é *SharedPreferences.getInstance()*, pois é o método que cria/recupera o arquivo contendo as informações persistidas. Ao executarmos esse método, caso não haja alguma pasta já existente referente a Shared Prefences, a mesma será criada, sendo um arquivo XML com os pares chave:valor persistidos. Além deste método, temos métodos para salvar dados, recuperar dados e remover dados. Vamos analisar cada um destes método através do código passado em sala pelo professor.

### Método para salvar dados no arquivo

Antes da execução de qualquer método nós precisamos chamar o método *SharedPreferences.getInstance()*, pois ele retornará o arquivo no qual nossos dados irão persistir. Por exemplo, considere o método abaixo:
```
  void _save() async{
    final prefs = await SharedPreferences.getInstance();

    String value = dataController.text;

    prefs.setString("data",value);
    print("Salvando $value");

  }
```

Considerações sobre o código acima:
* Primeiramente criamos uma conexão com o arquivo que nossos dados vão persistir atraves de *final prefs = await SharedPreferences.getInstance()*. Note que é uma função assíncrona, portanto precisamos adicionar await na frente.
* Em seguida criamos uma variável que receberá um valor passado por uma caixa de texto. 
* O método *prefs.setString("data",value)* é o responsável por adicionar um par chave:valor para o arquivo Shared Preferences, onde esse valor será inserido por um usuário.


### Método para recuperar dados do arquivo

O código utilizado par recuperar dados é semelhante ao de adicionar, portanto precisamos criar a conexão com o arquivo atraves de *SharedPreferences.getInstance()*. Após isso, podemos utilizar a variável que recebeu o documento (prefs) para recuperar os dados através do método *prefs.getString("chave")*. Podemos ter também *getBool*, *getInt*, *getStringList*, entre outros. Este método irá retornar o valor contido na chave passada como parâmetro. Segue abaixo a descrição dese código:
```
void _recover() async {
    final prefs = await SharedPreferences.getInstance();

    data = await prefs.getString("data")!;
    print("Recover: $data");

    setState((){});
  }
```

**OBS:** É necessário passar o setState no final do método paraa tela ser renderizada e o valor ser modificado na interface. 

### Método para remover dados do arquivo

Por último, podemos remover algum dado caso a gente passe uma chave existente como parâmetro para o método prefs.remove("chave"). Este método segue o mesmo principio dos outros, onde o método é responsável por abstrair o processo de remoção de um dado no arquivo. Segue abaixo o método delete:
```
void _delete() async {
    final prefs = await SharedPreferences.getInstance();

    await prefs.remove("data");
    
    print("Remove");
  }
```

## Resumo

Utilizar a API *Shared Preferences* pode ser bastante útil, pois esta API nos fornece maneiras fáceis de persistir dados em diferentes tipos de plataforma. É ideal para arquivos e pequenas configurações que precisam ser persistidas, como definir o tema de um aplicativo, ou configurações gerais. Os métodos fornecidos pela API simulam os métodos comuns de uma CRUD, abstraindo as partes complicadas do usuário e fornecendo uma interface simples e fácil de entender. 
