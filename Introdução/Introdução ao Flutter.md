# Flutter 
> Flutter é um framework de desenvolvimento de aplicativos criado pelo google

Através do Flutter podemos criar aplicações para web, desktop, mobile, através de uma única base de código. Sua linguagem padrão é Dart, que é orientada a objetos e otimizada para construir interfaces de usuário (UI). Aqui estão alguns pontos importantes sobre o Flutter:

### 1. Arquitetura de Widgets
No Flutter, a interface de usuário é construída utilizando uma árvore de widgets. Cada elemento visual, como texto, botões, ou algum layout, é representado por um widget, tornando a construção da interface altamente modular e reutilizável.

### 2. Hot Reload
É uma das características mais importantes do Flutter. O Hot Reload permite o desenvolvedor acompanhar das modificações da interface em tempo real, acelerando o processo de desenvolvimento.

### 3. Design Material e Cupertino
O Flutter oferece suporte a dois estilos de design principais: Material Design (inspirado no design da google) e Cupertino (inspirado no design da Apple). Isso permite que as aplicações possam ser criadas tanto para iOS quanto para Android.


O design Material será o utilizado para estudo de Flutter nessas anotações. Segue abaixo um exemplo geral de como se inicia uma aplicação em Flutter:
```
import 'package:flutter/material.dart'; 

void main() { 
	runApp(MaterialApp(title: "App", 
		home: Center(
			child: Text("Hello World!"),
		)
	)
); 
}
```

A porta de entrada para execução de um aplicativo é o método runApp. Dentro dele será passado como parâmetro o estilo de design desejado (no nosso caso MaterialApp). Dentro dos widgets nós temos atributos. Cada atributo é referente ao próprio widget que o chama, portanto, cada widget tem seus próprios atributos definidos. Alguns dos atributos de MaterialApp podem ser title, home, entre outros.

**OBS**: O atributo home é obrigatório. Ele mostra o que será exibido na tela principal.
