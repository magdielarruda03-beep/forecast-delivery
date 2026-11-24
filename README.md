# Forecast de Demanda para Delivery — Modelagem por Direcionadores no Excel

> Um forecast não serve para acertar o futuro. Serve para mostrar o que precisa acontecer para chegar lá, e o que acontece se não acontecer.

## Por que um analista de dados precisa saber fazer forecast

Boa parte do trabalho de dados olha para trás: quanto vendemos, qual produto girou mais, qual cliente sumiu. Isso é necessário, mas a decisão do gestor está sempre à frente. Quanto comprar, quantas pessoas escalar, quanto investir em marketing, se o caixa aguenta o próximo trimestre.

O forecast é a ponte entre a análise e a decisão:

- **Transforma histórico em hipótese testável.** "A frequência de compra é 2,5 pedidos por mês" vira "se a frequência cair 10%, quanto perco?".
- **Mostra quais variáveis importam.** Ao quebrar a receita em direcionadores (clientes, frequência, ticket, mix), fica claro onde está a alavanca e onde está o risco.
- **Cria conversa com o negócio.** Uma premissa explícita pode ser discutida, contestada e ajustada. Um número solto não.
- **Mede o custo de não agir.** O forecast deixa de ser só projeção e passa a precificar riscos: quanto vale um cliente que vai embora? Quanto vale convertê-lo em cadastro?

Quem domina forecast deixa de ser quem "puxa o relatório" e passa a ser quem ajuda a decidir.

## O projeto

Um modelo de forecast mensal para um delivery fictício, construído inteiramente no Excel. O modelo parte de dois anos de pedidos e projeta o ano seguinte por direcionadores. Ele inclui um cenário de risco: **o que acontece com a receita se os clientes não cadastrados deixarem de comprar?**

### Estrutura da planilha

| Aba | O que tem |
| --- | --- |
| **Forecast** | Painel com indicadores e gráficos, modelo mês a mês (clientes → pedidos → mix → receita → margem), perda de receita e grade de sensibilidade |
| **Premissas** | Histórico calculado direto da base, ajustes do analista e cenários de perda (pessimista, base, otimista) |
| **Base** | Pedidos de 2025 e 2026, um por linha: data, cliente, tipo de cliente, combo, itens, valor e desconto |

### Árvore de direcionadores

```
Receita = Pedidos totais × Ticket médio

Pedidos totais = Pedidos de cadastrados + Pedidos de não cadastrados
  Pedidos de cadastrados   = Base média de clientes × Frequência × Sazonalidade
  Base de clientes         = Base anterior + Novos − Churn
  Pedidos de não cadastrados = Mesmo mês do ano anterior × (1 + crescimento anual)

Ticket médio = (Pedidos com combo × Ticket combo + Pedidos avulsos × Ticket avulso) ÷ Pedidos

Margem de contribuição = Receita − CMV − Taxa da plataforma − Entregas − Cupons
```

### Cenário de perda: clientes não cadastrados

Cliente sem cadastro é receita sem dono: não dá para reativar, mandar cupom ou entender o comportamento dele. O modelo simula:

1. Qual % dos pedidos de não cadastrados deixa de acontecer
2. A partir de qual mês a perda começa
3. Quanto desses clientes é recuperado por ações de cadastro

Daí saem a receita perdida, a margem perdida e o % da receita em risco. Uma grade de sensibilidade cruza "% que deixa de comprar" com "% recuperado via cadastro". Ela mostra quanto vale, em reais, investir em cadastro.

## Decisões de modelagem

- **Premissas vêm dos dados, não do achismo.** Frequência, churn, ticket, mix de combos e sazonalidade são calculados da base com CONT.SES e SOMASES. O analista só ajusta por cima, em colunas separadas (Histórico | Ajuste | Forecast).
- **Dois anos de histórico.** Com um ano só, sazonalidade e crescimento se confundem. Com dois, o crescimento vem da comparação ano contra ano e a sazonalidade é a média dos dois anos.
- **Custos que não estão na base são digitados e sinalizados.** CMV, taxa da plataforma e custo de entrega ficam como premissas explícitas.
- **Padrão de cores de modelagem financeira.** Azul para o que é digitado, preto para fórmula, verde para link de outra aba.
- **Tudo recalcula.** Basta trocar a base, o cenário ou um ajuste: o modelo, o painel e os gráficos se atualizam.

## Como usar

1. Cole os pedidos reais na aba **Base**, mantendo as colunas.
2. Em **Premissas**, confira o ano de referência (B9) e preencha CMV, taxa e custo de entrega.
3. Ajuste as premissas nas colunas azuis e escolha o cenário de perda (C5: 1, 2 ou 3).
4. Leia o resultado na aba **Forecast**.

## Ferramentas

Excel: CONT.SES, SOMASES, ESCOLHER, SOMARPRODUTO, formatação condicional, validação de dados e gráficos.

## Sobre os dados

Os dados são fictícios, gerados pelo Claude (Anthropic) para fins de estudo e portfólio. Não representam nenhuma empresa real.

---

**Magdiel Arruda** · Analytics Engineer · [LinkedIn](https://www.linkedin.com/in/magdiel-arruda-42093322a) · [GitHub](https://github.com/magdielarruda03-beep)
