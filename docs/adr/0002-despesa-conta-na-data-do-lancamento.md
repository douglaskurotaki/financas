# Despesa conta na Data do lançamento; Pagamento de fatura não é Despesa

Todos os relatórios de Receitas e Despesas usam a Data do lançamento: uma compra no Cartão conta no dia da compra, e cada Parcela conta um mês depois da anterior. O Pagamento de fatura só tira dinheiro da Carteira e quita a Fatura; contá-lo como Despesa contaria a mesma compra duas vezes. O movimento real do dinheiro aparece no Saldo atual das Carteiras.

## Considered Options

- Compra parcelada inteira no mês da compra: concentra o gasto num mês só, reclamação frequente de usuários do Organizze com cartão conectado.
- Parcela no mês de vencimento da sua Fatura: empurra até compras à vista para o mês seguinte, ao contrário de como as pessoas pensam ("comprei em outubro").
- Dois regimes, como o Organizze (caixa num relatório, competência em outro): os totais divergem entre telas, e eles mantêm um artigo de ajuda só para explicar por quê.
