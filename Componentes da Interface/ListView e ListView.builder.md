# ListView e ListView.builder

> Widget de rolagem usado para exibir itens de uma lista.

Existem diferentes maneiras de criar um ListView, onde a escolha correta irá depender da aplicação. Listas dinâmicas ou muito grandes utilizam *ListView.builder*, enquanto listas menores e estáticas utilizam o *ListView*. 

## Principais atributos de ListView.builder

1. *itemCount:* Define o número total de itens na lista.
2. *itemBuilder:* Uma função de callback que é chamada para construir cada item da lista. Recebe como parâmetro  um context e um index. 

Segue um exemplo de como utilizar um ListView.builder:
```
ListView.builder(
	itemCount: postList!.lenght,
	itemBuilder: (contexnt, index){
		Post post = postList[index];

		return ListTile(
			leading: Text(post.id.toString),
			title: Text(post.title),
			subtitle: Text(post.userId.toString),
			onTap: (context){
				return AlertDialog(
					title: Text(post.title),
					content: Text(post.body,
					actions: [
						TextButton(
							onPressed:(){
								Navigator.pop(context);
							}
							child: Text("Fechar")
						),
					]
				);
			}
		);
		
	}
)
```
