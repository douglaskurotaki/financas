# Finanças

Gerenciador de finanças pessoais manual: a pessoa registra onde guarda o dinheiro, seus cartões de crédito e cada receita e despesa, e acompanha saldos, faturas e relatórios. "Finanças" é nome provisório.

## Language

### Quem usa

**Espaço** _(en: Space)_:
O conjunto de todos os dados financeiros de uma pessoa ou, no futuro, de um grupo que divide as finanças. Toda Carteira, Cartão, Categoria e Lançamento pertence a um Espaço.
_Avoid_: conta, perfil, workspace

**Usuário** _(en: User)_:
Pessoa que entra com e-mail e senha e trabalha dentro de um Espaço.
_Avoid_: cliente, conta, membro

### Onde o dinheiro está

**Carteira** _(en: Wallet)_:
Qualquer lugar onde o dinheiro fica guardado: conta corrente, poupança, conta digital ou dinheiro em espécie.
_Avoid_: conta, conta bancária

**Saldo inicial** _(en: initial balance)_:
Quanto havia na Carteira quando ela começou a ser acompanhada.

**Saldo atual** _(en: current balance)_:
Saldo inicial mais tudo o que já se moveu na Carteira até hoje: Lançamentos efetivados, Transferências, Pagamentos de fatura e Ajustes de saldo.

**Saldo previsto** _(en: projected balance)_:
Saldo atual mais as Receitas pendentes, menos as Despesas pendentes e as Faturas em aberto, até uma data.

**Ajuste de saldo** _(en: balance adjustment)_:
Correção que iguala o Saldo atual de uma Carteira ao saldo real. Não é Receita nem Despesa.
_Avoid_: lançamento de ajuste

**Transferência** _(en: Transfer)_:
Dinheiro movido de uma Carteira para outra do mesmo Espaço. Não é Receita nem Despesa.
_Avoid_: entrada, saída

### Receitas e despesas

**Lançamento** _(en: Transaction)_:
Uma Receita ou uma Despesa, numa Carteira ou num Cartão.
_Avoid_: movimentação

**Receita** _(en: income)_:
Lançamento que traz dinheiro para o Espaço: salário, venda, rendimento.
_Avoid_: entrada, ganho, crédito

**Despesa** _(en: expense)_:
Lançamento que tira dinheiro do Espaço, seja de uma Carteira, seja numa Fatura.
_Avoid_: saída, gasto, débito

**Data do lançamento** _(en: date)_:
O dia em que o Lançamento vence ou aconteceu. Define em que mês ele conta nos relatórios.
_Avoid_: competência, vencimento

**Pendente** _(en: pending)_:
Situação de um Lançamento de Carteira cujo dinheiro ainda não se moveu.
_Avoid_: não pago, a pagar, previsto

**Efetivado** _(en: settled)_:
Situação de um Lançamento de Carteira cujo dinheiro já se moveu. Na tela aparece "Pago" ou "Recebido". Lançamentos de Cartão não têm situação: quem fica em aberto ou paga é a Fatura.
_Avoid_: pago, realizado, confirmado

**Efetivado em** _(en: settled on)_:
O dia em que o dinheiro de um Lançamento de Carteira se moveu de fato. Define quando ele entra no Saldo atual. Na tela aparece "Pago em" ou "Recebido em".
_Avoid_: data de pagamento

**Categoria** _(en: Category)_:
Classificação de Receitas ou de Despesas, nunca das duas. Toda Receita e toda Despesa tem uma.
_Avoid_: tipo, grupo, tag

**Subcategoria** _(en: subcategory)_:
Categoria dentro de outra, com um nível só. Nos relatórios, soma na Categoria de cima.

### Cartão de crédito

**Cartão** _(en: CreditCard)_:
Cartão de crédito, com limite, Dia de fechamento e Dia de vencimento. Compra no débito não usa Cartão: é Despesa da Carteira.
_Avoid_: cartão de débito, conta do cartão

**Fatura** _(en: Invoice)_:
As Despesas e Estornos de um Cartão em um ciclo mensal, a pagar até o Dia de vencimento.
_Avoid_: extrato, conta do cartão

**Dia de fechamento** _(en: closing day)_:
Dia do mês em que a Fatura fecha. Compras feitas a partir dele vão, por padrão, para a Fatura seguinte.
_Avoid_: corte, melhor dia de compra

**Dia de vencimento** _(en: due day)_:
Dia do mês em que a Fatura deve ser paga.

**Limite disponível** _(en: available limit)_:
Limite do Cartão menos tudo o que ainda não foi pago, inclusive Parcelas de Faturas futuras.

**Pagamento de fatura** _(en: invoice payment)_:
Dinheiro que sai de uma Carteira para quitar, no todo ou em parte, uma Fatura. Não é Despesa: as Despesas são as compras.

**Estorno** _(en: refund)_:
Crédito numa Fatura que abate Despesas: devolução de compra, cashback, anuidade devolvida. Fica na Categoria da Despesa que abate e nunca é Receita.
_Avoid_: reembolso, devolução

**Parcelamento** _(en: installment plan)_:
Compra no Cartão dividida em Parcelas mensais, uma em cada Fatura.
_Avoid_: compra parcelada, série

**Parcela** _(en: installment)_:
Cada parte de um Parcelamento, numerada sobre o total (ex.: 3/12). É uma Despesa cuja Data do lançamento fica um mês depois da Parcela anterior.
_Avoid_: prestação
