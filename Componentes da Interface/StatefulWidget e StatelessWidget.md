# Stateful Widget e Stateless Widget
> No Flutter, os componentes da interface são representados por Widgets.

Através dos widgets nós conseguimos representar elementos visuais na tela. Eles podem ser simples, como textos, botões, ou complexos, como layouts da tela inteira. Os widgets são organizados em uma estrutura hierárquica chamada “árvore de widgets”. Cada widget pode conter dentro de si outros widgets, criando uma árvore que compõe os widgets.

**OBS:** Geralmente, o widget raiz é o _MaterialApp_ ou _CupertinoApp_.

Os widgets são declarados como classes em Dart. Existem dois tipos principais de widgets:

1. **StatelessWidget:** Widgets que não mantém estado. Eles são imutáveis e sua interface depende apenas das propriedades recebidas.
2. **StatefulWidget:** Widgets que podem mudar de estado. Eles são divididos em duas classes: a classe principal do widget e a classe de estado, onde a lógica e o estado são mantidos.

Segue abaixo um exemplo de como aninhar componentes dentro de um _StatelessWidget_:
```
class myApp extends StatelessWidget{ 
	@override 
	Widget build(BuildContext context){ 
		return Scaffold(
			appBar: AppBar( title: Text("App base") ), 
			body: Center( 
				child: Column( 
					children: [ 
						Text("Bem vindo ao flutter!"), 
						ElevatedButton( 
							onTap: (){}, 
							child: Text("Clique aqui!") 
							) 
						] 
					) 
				), 
			); 
		} 
	}
```

> StatelessWidget e StatefulWidget são os tipos principais de componentes da interface.

## Stateless Widget

Um StatelessWidget é um tipo de Widget que não possui estado interno que possa mudar. Ele é construído apenas uma vez e sua aparência e comportamento são definidos pelas propriedades que recebem. Utilizamos StatelessWidget quando a interface não precisa ser modificada ao realizar interações com o usuário.

Algumas características do StatelessWidget são:

1. **Imutabilidade**: O estado do widget é imutável, ou seja, uma vez que ele é construído, não pode ser alterado. Se for necessário realizar atualizações na interface, será preciso criar um novo widget com novas propriedades.
2. **Construção Única:** O método _build_ é chamado apenas uma vez, quando o widget é inserido na árvore de widgets.
3. **Desempenho:** Como os StatelessWidget não mantem estado, eles geralmente são mais leves e têm melhor desempenho em comparação com os StatefulWidget.

Utilizamos StatelessWidget nas seguintes situações:

**Sem estado:** Quando a interface não dependa de informações que possam mudar durante a execução do aplicativo

**Componentes Estáticos:** Quando criamos componentes que são essencialmente estáticos, como títulos, textos, ícones ou imagens.

Todo _StatelessWidget_ possui uma estrutura básica, na qual algumas informações devem estar presentes, sendo elas:

- **Classe:** A classe deve estender _StatelessWidget_
- **Método build:** O método _build_ deve retornar a árvore de widgets que representa a interface.

Segue abaixo um exemplo detalhado de um StatelessWidget:
```
import 'package:flutter/material.dart'; 

void main(){ 
	runApp( 
		MaterialApp( 
			title: "App base" 
		) 
	); 
} 

class appBase extends StatelessWidget{ 
	@override 
	Widget build(BuildContext context){ 
		return Scaffold( 
			appBar: AppBar(title: Text("App Bar"), 
			body: Center( 
				child: Column( 
					mainAxisAlignment: MainAxisAlignment.spaceEvenly, 
					children: [ 
						Text("Texto 1"), 
						ElevatedButton( 
							onTap: (){}, 
							child: Text("Clique!") 
						),
					] 
				), 
			), 
		); 
	} 
}
```

A classe possui um método build, que é chamado para criar a interface do widget. O retorno desse método é um componente isolado ou uma estrutura completa da página.

## Stateful Widget

>StatefulWidget é um tipo de widget que pode manter um estado que muda ao longo do tempo.

Widgets do tipo StatefulWidget podem ser reconstruídos quando o estado interno do widget é alterado, permitindo que ele reflita essas mudanças na interface do usuário sem a necessidade de recriar inteiramente o widget modificado.

A estrutura básica de um _StatefulWidget_ consiste em duas principais classes:

1. **Classe StatefulWidget**: Essa é a classe que define o widget em si. Ela é imutável e, portanto, não mantém um estado interno.
2. **Classe State**: Esta classe é responsável por manter o estado do widget e fornecer a lógica para atualizar a interface do usuário quando o estado muda.

Vamos exemplificar como cada parte pode ser declarada na criação de um StatefulWidget:
```
class WidgetPrincipal extends StatefulWidget{ 
	State<WidgetPrincipal> createState() => 
}
```

Dentro da classe StatefulWidget precisamos implementar o método createState(). Esse método será do tipo State< WidgetPrincipal>, portanto, precisamos passar o estado atual da nossa aplicação. O estado atual precisa estar numa classe a parte. Essa classe herdará de State< WidgetPrincipal>. Segue abaixo a estrutura dessa classe e como ela é implementada:

```
class _WidgetPrincipalState extends State<WidgetPrincipal>{ 
	int _contador = 0; //Variável de estado 
	Widget build(BuildContext context){ 
		return Scaffold( 
			appBar: AppBar(title:Text("App bar")), 
			body: Center( 
				child: Column( 
					mainAxisAlignment: MainAxisAlignment.spaceEvenly, 
					children: [ 
						Text("Número de cliques:"), 
						Text("$_contador"), 
						ElevatedButton( 
							onTap: (){ 
								setState((){ 
									_contador++; 
								}); 
							}, 
							child: Text("Clique aqui") 
						) 
					] 
				), 
			), 
		); 
	} 
}
```

**OBS:** O método setState() é essencial, pois ele avisa ao Flutter que uma alteração foi feita, permitindo que essa alteração seja exibida em tempo real.

Dentro do método setState() nós modificamos nossa variável de estado. Em todo StatefulWidget nós teremos variáveis de estado, sendo essas as responsáveis pelas mudanças dinâmicas que ocorrem na página. Após setState ser executado, o método build é executado novamente, reconstruindo a interface e exibindo as modificações feitas.
