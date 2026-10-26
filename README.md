# Finanças

Gerenciador de finanças pessoais manual: carteiras, cartões de crédito com fatura e parcelamento, receitas e despesas por categoria, e relatórios. MVP em construção; "Finanças" é nome provisório.

## Documentação

- [`CONTEXT.md`](CONTEXT.md): glossário do domínio (Espaço, Carteira, Fatura, Parcela…), usado em conversas, código e documentação.
- [`docs/adr/`](docs/adr/): decisões de domínio e arquitetura.
- [`docs/research/`](docs/research/): pesquisas de referência (funcionalidades do Organizze e Open Finance no Brasil).

## Stack

Ruby 4.0.9, Rails 8.1.4, PostgreSQL, Hotwire, Tailwind CSS e Minitest.

## Rodando localmente

Requer Ruby 4.0.9 e um PostgreSQL rodando.

```sh
bin/setup       # instala as gems, prepara o banco e sobe o servidor (--skip-server para não subir)
bin/dev         # servidor e Tailwind em modo watch, em http://localhost:3000
bin/rails test  # testes
```
