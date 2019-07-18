---
layout: post
title: O jeito certo de ler um arquivo em Python
description: Existem muitos exemplos te ensinando a ler um arquivo em Python. O que há de errado com eles?
featured-img: blur-children-class-448877
categories: [pt-br]
lang: pt_BR
---

Existe um mundo de exemplos por aí te ensinando um jeito errado de ler um arquivo em Python.

Afinal de contas, qual é o problema?

A maioria dos exemplos não fecha o arquivo quando ocorre um erro e alguns deles nunca fecham o arquivo.

Frequentemente eles confiam que o Python vai fechar o arquivo quando o programa terminar, mas se você se acostumar a copiar esses exemplos, alimentará o péssimo hábito de abrir arquivos de uma forma insegura.

Dito isto, a maneira correta de abrir qualquer arquivo em Python é:


```
with open("input.txt") as f:
    for line in f:
        # handle "line"
        # ...

# here the file is already closed.
```

Esse é o jeito certo de ler um arquivo em Python porque é garantido que ele será fechado, mesmo se ocorrer algum erro.

O comando `with` é um _context processor_ e sempre fecha o arquivo. Mesmo se você sair dele com um `break` ou lançando uma exceção.
