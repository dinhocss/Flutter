# Expanded

> É utilizado em conjunto de Row, Column ou Flex, tornando o widget filho flexível em relação aos outros filhos.

Utilizamos o expanded para tornar o widget filho de um Column ou Row como flexível, permitindo com que ele ocupe o espaço disponível no eixo, respeitando o atributo definido como flex.

## Propriedades do Expanded

1. *child*: 
	* O widget filho que será expandido.
	* Pode ser um container, um text, ou qualquer outro widget de interesse.
2. *flex*:
	* Um inteiro que define a proporção de espaço que um filho terá em relação aos outros.
	* Por exemplo, um widget filho com flex: 2 terá o dobro de espaço em relação a um widget filho com flex: 1

Segue abaixo um exemplo de como utilizar o expanded:
```
Row(
  children: [
    Expanded(
      child: Container(color: Colors.red),
    ),
    Expanded(
      child: Container(color: Colors.blue),
    ),
  ],
)
