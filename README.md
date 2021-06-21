# Planejamento de Distribuição e Produção

Este exercício para o processo seletivo da OpsFactor consiste na construção de uma aplicação Java capaz de, a partir de pedidos de clientes, definir os volumes de produção e distribuição necessários para o atendimento da demanda.

O candidato deverá desenvolver o sistema com base:
* Nas definições de relacionamento entre clientes, pedidos, ordens de produção, linhas de produção e armazéns
* Na especificação da empresa : instâncias das classes acima (valores a serem populados no banco de dados)

As soluções serão avaliadas com base em 2 critérios : stack utilizada e raciocínio lógico do candidato.

Caso a entrega seja feita parcialmente levaremos em consideração o trabalho feito pelo candidato.

## Notas para avaliação da stack:
3 : Aplicação Java / Spring + dados em banco de dados + acesso aos dados via ORM (entidades/repositórios, Hibernate/JPA) + APIs com controllers Spring

2 : Aplicação Java + dados em banco de dados (tipicamente usamos MySQL) + acesso aos dados via DAO (queries SQL) + APIs com servlets / JAX

1 : Aplicação Java + dados em mapas ou outra estrutura de dados 'hard-coded' + APIs com servlets / JAX


## Pontos considerados na avaliação do raciocínio lógico:
* Capacidade de modelagem das classes/entidades, com relacionamentos coerentes com o problema apresentado
* Desenvolvimento dos cálculos para sugestão de cada uma das decisões
* Características gerais do código : uso correto de loops e streaming (se necessário), organização e coerência


## Estrutura de Dados

### Cliente

Cada cliente deverá ter um identificador (cnpj), uma descrição e uma prioridade.
Os clientes considerados são:
1) Cliente A (cnpj 5000) , com prioridade 3
2) Cliente B (cnpj 7000) , com prioridade 2
3) Cliente C (cnpj 3000) , com prioridade 1

### Produto

Os produtos poderão ser solicitados por cada um dos clientes e deverão ser produzidos e distribuídos
Os produtos considerados são:

1) Produto X
2) Produto Y

### Carteira de Pedidos

Os clientes podem emitir uma carteira de pedidos para nossa empresa, que deverá atendê-los através de um fluxo de produção e distribuição

Temos os seguintes pedidos em carteira:
1) Pedido 1 : produto X, cliente A, quantidade = 2000
2) Pedido 2 : produto Y, cliente A, quantidade = 3000
3) Pedido 3 : produto X, cliente B, quantidade = 1500
4) Pedido 4 : produto Y, cliente B, quantidade = 1500
5) Pedido 5 : produto X, cliente C, quantidade = 2000
6) Pedido 6 : produto Y, cliente C, quantidade = 2000

### Unidades (fábricas ou armazéns)

Trabalharemos com 3 unidades diferentes : 
* Fábrica F1
* Armazém A1
* Armazém A2

O caminho que o produto deve percorrer até o cliente é:

Cliente A: Produção na Fábrica F1, Transporte para Armazém A1, Expedição para cliente A
Cliente B: Produção na Fábrica F1, Transporte para Armazém A2, Expedição para cliente B
Cliente C: Produção na Fábrica F1, Transporte para Armazém A2, Expedição para cliente C

Em resumo, o armazém A1 expede somente para o cliente A e o armazém A2 expede para os clientes B e C

### Linhas de Produção

A fábrica possui duas linhas de produção : uma para o produto X e outra para o produto Y

* Linha de produção para produto X : LP-X
* Linha de produção para produto Y : LP-Y

### Ordens de Produção

O output do programa serão ordens de produção, que dizem que produto em que quantidade deve ser produzido em que linha de produção

Exemplo : Produção de 5000 unidades do produto X na linha LP-X

### Ordens de Transferência

Indicam que quantidades de cada material devem ser enviadas a cada um dos armazéns

Exemplo : Transferência de 3000 unidades do produto Y da fábrica F1 para o armazém A2

### Ordens de Expedição

Indicam que pedido será atendido em que quantidade a partir de qual armazém

Exemplo: Pedido 50 - Armazém A2 - quantidade atendida = 500


## Funcionamento do Programa

O programa deverá , em sequência :
1) Determinar que quantidades deverão ser expedidas de cada armazém para cada cliente, com base na carteira (gerar ordens de expedição)
2) Determinar que quantidades deverão ser transferidas para cada armazém da fábrica, a partir das ordens de expedição (gerar ordens de transferência)
3) Determinar as quantidades a serem produzidas por máquina, a partir das ordens de transferência (gerar ordens de produção)

Precisaremos então, em uma interface gráfica simples (web), de uma tela com dois botões:

Botão 1 : gerar as ordens e salvá-las no banco de dados (apaga as sugestões antigas e cria novas)
Botão 2 : apresentar as sugestões. Deverá mostrar as ordens de produção, transferência e expedição em 3 tabelas distintas

## Desafio Adicional (Opcional)
* Considerar que o armazém A1 já tenha um estoque disponível de 500 unidades para o produto X e 300 unidades do produto Y. cálculo de transferência e produção devem considerar esses valores
* Criar para cada linha de produção um campo de capacidade produtiva máxima (4500 para produto X e 7000 para produto Y). caso a capacidade de linha seja excedida, os clientes de maior prioridade deverão receber antes e o saldo faltante será retirado de parte da carteira dos clientes menos prioritários (sugestão : ordenar carteira por prioridade e atender na sequência até acabar a disponibilidade de capacidade + estoques)
