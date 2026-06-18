# Projeto de Padroes de Projeto em JavaScript

Projeto feito em Node.js para as atividades de laboratorio sobre padroes de projeto. O sistema simula um backend simples de e-commerce, com pedidos, pagamentos, checkout, carrinho, frete e notificacoes.

## Alunos

- Arthur Henrique Abreu
- Matheus Fonte Goncalves

## Como executar

```bash
npm start
```

Ou:

```bash
node src/index.js
```

O arquivo `src/index.js` executa exemplos de todos os padroes implementados.

## Resumo do projeto

A ideia do trabalho foi aplicar padroes de projeto em um e-commerce para deixar o codigo mais organizado, reutilizavel e facil de alterar. A Atividade 03 usa padroes criacionais para criar objetos de forma controlada. A Atividade 04 evolui o projeto com padroes estruturais e comportamentais.

## Padroes usados e justificativas

### Singleton - Conexao

A classe `Conexao` garante que exista apenas uma instancia de conexao com o banco durante a execucao.

**Justificativa:** faz sentido usar Singleton porque a conexao e um recurso compartilhado. Ele evita criar varias conexoes desnecessarias e centraliza o acesso ao banco.

### Factory Method - Pagamentos

A `PagamentoFactory` cria pagamentos do tipo PIX, boleto ou cartao de credito sem o codigo principal conhecer as classes concretas.

**Justificativa:** se for necessario adicionar uma nova forma de pagamento, como criptomoeda, basta criar uma nova classe e ajustar a factory. O restante do sistema continua usando o mesmo contrato `processar(valor)`.

### Builder - Pedido

O `PedidoBuilder` monta um pedido passo a passo, adicionando itens, endereco e forma de pagamento antes de criar o objeto final.

**Justificativa:** Builder e melhor que um construtor com muitos parametros porque deixa a criacao mais legivel e permite validar o pedido antes de finalizar.

### Adapter - Gateway legado

O `GatewayAdapter` permite usar o `GatewayLegado`, que possui uma interface diferente, sem mudar o codigo do pedido ou do pagamento.

**Justificativa:** sem Adapter, o sistema teria que conhecer diretamente a API do gateway legado. Com Adapter, a integracao e adicionada sem alterar o codigo existente, respeitando o principio Open/Closed.

### Decorator - Log e desconto

`LogDecorator` e `DescontoDecorator` adicionam log e desconto ao pagamento sem alterar as classes PIX, boleto ou cartao.

**Justificativa:** novos comportamentos, como envio de SMS, podem ser adicionados criando outro decorator. Isso e melhor que heranca simples, porque evita criar muitas subclasses para cada combinacao de comportamento.

### Facade - Checkout

A `CheckoutFacade` centraliza o fluxo de finalizar pedido, chamando estoque, pagamento, carrinho e email.

**Justificativa:** sem Facade, o controller teria que conhecer todos esses servicos. Se algum servico mudasse sua API, o controller tambem teria que mudar. A Facade protege o codigo cliente dessas mudancas internas.

### Strategy - Frete

O `Carrinho` usa estrategias de frete como `FreteCorreios`, `FreteJadlog` e `FreteRetirada`.

**Justificativa:** para adicionar uma nova transportadora, como DHL, basta criar uma nova estrategia com o metodo `calcular(peso)`. O carrinho nao precisa ser alterado. Esse padrao ajuda a respeitar o Open/Closed e o Single Responsibility Principle.

### Observer - Notificacoes

A classe `Pedido` notifica observadores quando muda de status, como email, estoque e log.

**Justificativa:** para adicionar uma notificacao por SMS, basta criar um novo observer e registra-lo no pedido. Sem Observer, `Pedido` teria que chamar cada servico diretamente, ficando muito acoplado.

### Command - Acoes com undo

Os comandos encapsulam acoes como cancelar pedido e atualizar endereco. O `GerenciadorComandos` guarda historico e permite desfazer a ultima acao.

**Justificativa:** alem do undo, Command ajuda a auditar acoes, criar historico e enfileirar tarefas. Em uma fila assincrona, cada tarefa poderia ser um comando executado depois por um worker.

## Estrutura principal

```text
src/
  index.js
  database/
  pagamentos/
  pedidos/
  checkout/
  carrinho/
  frete/
  observadores/
  comandos/
```

## Conclusao

O projeto mostra como os padroes de projeto reduzem acoplamento e facilitam manutencao. Com eles, o sistema pode receber novos pagamentos, fretes, notificacoes e comandos com menos alteracoes nas classes ja existentes.
