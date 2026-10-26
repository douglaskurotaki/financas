# Open Finance, agregadores e alternativas: como um app de finanças pessoais pode ler dados bancários no Brasil (out/2026)

Pesquisa feita em 26/10/2026, só com fontes públicas. O objetivo é dar fatos para duas decisões: se o MVP começa com conexão bancária automática (Open Finance) ou manual, e como precificar depois. Complementa o [mapa do Organizze](organizze-funcionalidades.md) (seções 4.13, 6.5, 7 e 8).

## Como ler este documento

- **[P]** = fonte primária: norma do BCB/CMN/ANPD, especificação oficial do Open Finance Brasil, página de preços, termos ou documentação do próprio fornecedor, política oficial da Apple ou do Google, central de ajuda oficial.
- **[S]** = fonte secundária (notícia, blog de terceiros, relato de desenvolvedor). Serve só como pista.
- **(não verificado)** = não consegui confirmar.
- **Alegação do fornecedor** = número que a empresa divulga sobre si mesma.
- **Cálculo nosso / inferência** = conta ou leitura nossa a partir das fontes; não é afirmação da fonte.
- **Método.** As normas do BCB foram lidas pela API de normativos do BCB, porque as páginas HTML são montadas por JavaScript. A documentação do Open Finance Brasil foi lida pela API do Confluence e pelos repositórios oficiais no GitHub. As centrais de ajuda Zendesk (Organizze, Belvo) foram lidas pela API pública do Help Center. Também li o JavaScript público servido pela página de login do Organizze. Não fiz cadastro, não pedi demo e não contatei vendas.
- **Data importante.** A **Resolução Conjunta nº 21, de 22/10/2026** (4 dias antes desta pesquisa) reescreveu as regras de parceria que sustentam o modelo "agregador + app não regulado". O BCB ainda vai publicar requisitos complementares, então partes da seção 5.1 podem mudar.

---

## 1. Resumo

### 1.1 O que é viável para um fundador solo

| Caminho | Situação para fundador solo | Fato que define |
|---|---|---|
| Ser participante direto do Open Finance | **Inviável** | Só instituição autorizada pelo BCB recebe dados. O caminho usado pelos agregadores (instituição de pagamento na modalidade iniciadora, ITP) passou a exigir capital mínimo de ~R$ 5,6 milhões, ou ~R$ 10,6 milhões se a instituição agregar dados do Open Finance (cálculo nosso sobre a RC 14/2025 e a Res. BCB 517/2025; antes era R$ 1 milhão). Exige ainda diretoria responsável, compliance, certificação FAPI e também expor APIs como transmissora (seção 2.2). |
| Agregador regulado, com o app como "entidade parceira" | **Viável, com custo fixo mensal e empresa constituída** | Menor preço público entre agregadores com licença própria: Belvo Launch R$ 1.500/mês (só para alguns segmentos). Pluggy: a partir de R$ 2.500/mês, com até 500 conexões ativas. Revenda Polp (sobre a Celcoin): R$ 159/mês + R$ 3,60 a R$ 2,00 por consentimento ativo. Desde 22/10/2026, a RC 21 exige que um auditor independente registrado na CVM ateste o app ao menos uma vez por ano (custo não publicado) (seções 3 e 5.1). |
| Contas "de uso pessoal" (MeuPluggy, Banco MCP) | **Não serve para produto** | MeuPluggy: "Uso comercial exige o plano pago" ([P](https://www.pluggy.ai/meu-pluggy)). |
| Screen scraping (login e senha do usuário) | **Em retração** | Não achei vedação expressa do BCB para quem está fora do Open Finance. Mesmo assim, a Pluggy está descontinuando conectores diretos de pessoa física e a Belvo só lista conexões via Open Finance no Brasil (seção 2.4). |
| Importar arquivo (OFX/CSV/PDF) | **Viável** | Não há barreira regulatória específica. OFX de conta só está confirmado em fonte oficial para Nubank e Inter. Não achei fatura de cartão em OFX em nenhum dos 12 bancos pesquisados (seção 2.5). |
| Ler notificações dos apps de banco (Android) | **Viável, mas frágil** | É permitido via NotificationListenerService, com acesso concedido pelo usuário, e Organizze e Mobills já fazem isso. A captura depende do texto de cada banco (seção 2.6). |
| Ler SMS (Android) | **Incerto** | A permissão é restrita. A exceção "SMS-based money management" é temporária, passa por revisão e só vale quando não há alternativa (seção 2.6). |
| Captura automática no iPhone | **Só com automação montada pelo usuário** | O iOS não dá acesso a SMS nem a notificações de outros apps. Resta o app Atalhos (gatilhos "Mensagem" e "Transação") chamando uma ação do nosso app (seção 2.6). |

### 1.2 Fatos que mais pesam na decisão

1. **Custo unitário com preços públicos (cálculo nosso).**
   - Pluggy: R$ 2.500 ÷ 500 conexões = R$ 5 por conexão/mês se a franquia for toda usada. Com 50 conexões, dá R$ 50 por conexão.
   - Polp Starter com 50 consentimentos: R$ 159 + 50 × R$ 3,60 = R$ 339/mês, ou ≈ R$ 6,78 por consentimento.
   - Conexão/consentimento é por banco: um usuário com 3 bancos conta 3.
   - Para comparar, os planos conectados do Organizze custam R$ 49–79/mês ([documento anterior](organizze-funcionalidades.md)).
2. **Gratuidade só para testar.**
   - Pluggy: teste de 15 dias em produção sem cartão e sandbox sem limite de tempo. Ao fim do teste, "As conexões com contas reais de clientes pausam até você ativar um plano" ([P](https://www.pluggy.ai/precos)).
   - Belvo: sandbox grátis "apenas para fins de teste e avaliação". Programa para startups com "R$ 25 mil em créditos" para empresas "com até 250 clientes", com seleção e "por tempo determinado" ([P](https://belvo.com/pt-br/startups/)).
3. **A RC 21/2026 cria obrigações diretas para o app parceiro** ([P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21)):
   - consentimento expresso "relativamente a cada entidade parceira", dado no ambiente do agregador;
   - auditor independente registrado na CVM, no mínimo anualmente;
   - proibição de repassar os dados a terceiros e de fazer engenharia reversa;
   - eliminação dos dados ao fim da finalidade;
   - nome do app sem "termos característicos" de instituição financeira ou de pagamento;
   - contratos existentes informados ao BCB até 01/12/2026.
4. **Parcelas de cartão: o Open Finance de hoje não entrega o que os usuários do Organizze pedem** (seção 4.5).
   - Na API de cartão estável (v2.3.1), as faturas são só as fechadas. As transações trazem o número da parcela e o total de parcelas, mas não as parcelas futuras.
   - A v2.4.0 (especificação final, abr/2026) acrescenta `billForecastDate`, o mês em que cada parcela será faturada, inclusive as futuras. A data em que vira obrigatória e quantos bancos já a implementam não foram verificadas.
   - Hoje o Organizze diz que só Nubank, Itaú e Mercado Pago trazem faturas futuras ([P](https://ajuda.organizze.com.br/hc/pt-br/articles/34229891699475)).
5. **A frequência de atualização tem teto regulatório.** Os limites mínimos são mensais, por cliente, endpoint e instituição receptora: transações recentes 240, saldo 420, faturas 30, histórico 8 ([P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/17957025)). Quem estoura o limite fica sem dados novos até o mês seguinte (docs da [Pluggy](https://docs.pluggy.ai/en/docs/open-finance/rate-limits) e da [Belvo](https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-data-retrieval-limits)).
6. **Atraso.** A norma admite defasagem de até 5 minutos para saldo e transações de conta, e de até 1 hora nos demais casos ([P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=32)).
   - Pluggy: "new transactions can take up to 24h" ([P](https://docs.pluggy.ai/en/docs/get-started/faq)).
   - Organizze: "A instituição bancária tem até 48h" ([P](https://ajuda.organizze.com.br/hc/pt-br/articles/34229891699475)).
   - Transações de cartão só aparecem "após conciliado" ([P](https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/credit-cards/2.3.1.yml)).
7. **Consentimento.** O teto de 12 meses caiu com a RC 7/2023. Pluggy e Belvo pedem prazo indeterminado por padrão. A renovação sem redirecionamento existe enquanto o consentimento está ativo; depois que expira, o usuário precisa passar de novo pelo banco (seção 4.2).
8. **Transferências entre contas próprias são detectáveis pelos dados.** A API de contas traz o CPF/CNPJ da contraparte, obrigatório desde 02/05/2023 em transações de pagamento, e o tipo (PIX, TED, transferência na mesma instituição) ([P](https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/accounts/2.4.2.yml)). A Pluggy já categoriza "Same person transfer" (seção 4.6).
9. **Falhas de dados vêm também dos bancos.** Em 26/10/2026 o status público da Pluggy tinha 10 incidentes de dados abertos, entre eles "Inter | Compras parceladas sendo retornadas incorretamente" e "Bradesco - Transações de conta corrente não sendo retornada" ([P](https://status.pluggy.ai/api/status)).
10. **O Organizze usa a Belvo** em produção: a central de ajuda de set–out/2026 e o JavaScript atual do app confirmam. Os campos `pluggy_*` da API dele não têm explicação pública (seção 6).
11. **Lojas de apps.** A App Store diz que apps de "banking and financial services" ou que pedem informação sensível "should be submitted by a legal entity… and not by an individual developer" ([P](https://developer.apple.com/app-store/review/guidelines/)). Se isso pega um app de orçamento: (não verificado).

---

## 2. Caminhos de acesso aos dados

### 2.1 Tabela comparativa

| Caminho | Como funciona | Requisitos | Custo | Viabilidade (fundador solo) | Fontes |
|---|---|---|---|---|---|
| **(a) Participante direto do Open Finance** | A empresa vira instituição autorizada pelo BCB, entra como receptora de dados, pede o consentimento e chama as APIs dos bancos diretamente. | Autorização do BCB (caminho típico: IP na modalidade ITP); capital mínimo da RC 14/2025; diretor responsável, relatório e envolvimento do conselho/diretoria; adesão à convenção e ao Diretório; certificados ICP-Brasil; certificação FAPI; interface própria como transmissora de dados; PLD e cibersegurança. | Capital mínimo ~R$ 5,6–10,6 mi (cálculo nosso); certificação FAPI US$ 5.000 por implantação para não membros; contribuição mensal à Estrutura de Governança (valor em R$ não verificado); prazo de autorização sem máximo oficial encontrado. | **Inviável** | [RC 1/2020 consolidada](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf), [RC 14/2025](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=14), [Res. BCB 517/2025](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=517), [OpenID fees](https://openid.net/certification/fees/) |
| **(b) Agregador regulado** ("instituição integradora") | O agregador, uma IP autorizada com papel de dados no Diretório, obtém o consentimento no próprio widget. Depois recebe os dados dos bancos e entrega ao app por API e webhooks. | Empresa constituída (CNPJ explícito na Polp e no Iniciador; "empresa" na Belvo; due diligence de compliance na Pluggy); contrato de parceria com as cláusulas mínimas da RC 21; auditoria independente anual do app; consentimento por parceiro; nome do app sem termos de instituição financeira. | Piso público de R$ 1.500/mês (Belvo Launch) a R$ 2.500/mês (Pluggy, até 500 conexões), mais variável; revenda Polp a partir de R$ 159/mês + R$ 3,60–2,00 por consentimento; demais sob consulta; custo do auditor não publicado. | **Viável** | Seção 3; [RC 21/2026](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21) |
| **(c) Screen scraping** | O usuário entrega login e senha, e um robô lê o internet banking ou o app. | Nenhuma vedação expressa encontrada nas normas do BCB para quem está fora do Open Finance; LGPD se aplica; termos dos bancos responsabilizam o cliente pelo uso da senha. | Desenvolvimento e manutenção próprios (não medido). Agregadores estão saindo desse modelo. | **Em retração**; risco operacional e de termos de uso | Seção 2.4 |
| **(d) Arquivo** (OFX, CSV, PDF) | O usuário baixa o extrato ou a fatura no banco e importa no app. | Nada específico além da LGPD (é o usuário quem entrega o arquivo). | Só desenvolvimento (parsers de OFX, CSV e PDF). | **Viável**; cobertura de OFX irregular | Seção 2.5 |
| **(e) Notificações (Android)** | O app lê as notificações dos apps de banco (inclusive SMS exibidos) com NotificationListenerService. | O usuário concede "Acesso às notificações"; política de Dados do Usuário do Google Play (divulgação em destaque, Data safety). | Desenvolvimento e manutenção de padrões por banco. | **Viável, frágil** | Seção 2.6 |
| **(e) SMS (Android)** | O app lê os SMS dos bancos com READ_SMS/RECEIVE_SMS. | Só app padrão de SMS, ou a exceção temporária "SMS-based money management", sujeita a aprovação e a formulário. | Desenvolvimento. | **Incerto** | Seção 2.6 |
| **(e) iOS** | O iOS não deixa ler SMS nem notificações de outros apps. A alternativa é o app expor uma ação (App Intents) e o usuário criar uma automação no Atalhos com gatilho "Mensagem" ou "Transação". | O usuário precisa configurar a automação. | Desenvolvimento. | **Limitado** | Seção 2.6 |

### 2.2 (a) Participante direto: o que é exigido

- **Quem pode.** O Open Finance é implementado por "instituições financeiras, instituições de pagamento e demais instituições autorizadas a funcionar pelo Banco Central do Brasil" (RC 1/2020, art. 1º) [P](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf).
  - Participar voluntariamente como receptora "pressupõe a disponibilidade de interface dedicada… na condição de instituição transmissora de dados" (art. 6º, § 3º). Ou seja, quem recebe dados também precisa expor os seus.
- **Licença usada pelos agregadores.** No Diretório oficial, Pluggy, Belvo e Klavi aparecem como instituições de pagamento com papéis "DADOS" e "PAGTO" [P](https://data.directory.openbankingbrasil.org.br/participants).
  - A Belvo diz ser "autorizada a atuar como agregador de dados no âmbito do Open Finance e a iniciar transações de pagamento" [P](https://belvo.com/pt-br/politica-de-privacidade/).
  - Os termos da Polp descrevem a Pluggy como "Iniciadora de Transação de Pagamento (ITP) autorizada nos termos da Resolução BCB nº 80/2021" [P](https://polp.com.br/termos).
- **Requisitos para autorização** (Res. BCB 81/2021, art. 2º) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=81):
  - capacidade econômico-financeira dos controladores;
  - origem lícita do capital;
  - modelo de negócio sustentável;
  - TI e governança compatíveis;
  - reputação e capacitação dos administradores;
  - capital mínimo.
  - O BCB pode convocar para entrevista (art. 5º).
- **Capital mínimo.** Antes, a ITP precisava de R$ 1 milhão (Res. BCB 80/2021, art. 17, II [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=80)). Esse valor foi substituído pela metodologia da RC 14/2025, em vigor desde a publicação (03/11/2025) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=14).
  - A transição gradual (25%/50%/75% até 2027) só vale para quem já funcionava ou já tinha pedido protocolado.

| Componente (RC 14/2025, arts. 8º a 10; Res. BCB 517/2025, arts. 3º e 4º) | Regra | ITP sem agregação de dados | ITP que agrega dados do Open Finance ou repassa a parceiros |
|---|---|---|---|
| Parcela de custo | R$ 2 milhões × nº de categorias de atividade (a ITP está na categoria "serviços"). Soma-se R$ 5 milhões se a instituição presta serviço intensivo em TI; a Res. BCB 517, art. 4º, inclui nesse grupo "a agregação de dados compartilhados no âmbito do Open Finance" e "o compartilhamento de dados com entidades não autorizadas… conforme contratos de parceria". | R$ 2 mi | R$ 7 mi |
| Parcela de atividades | (R$ 1 mi da categoria serviço + R$ 5 mi da categoria de investimento "restrita" ou R$ 8 mi da "livre") × fator de captação (60% para "recursos próprios") | R$ 3,6 mi | R$ 3,6 mi |
| **Total (cálculo nosso)** | | **~R$ 5,6 mi** | **~R$ 10,6 mi** |

O cálculo supõe investimento "restrito" e captação por "recursos próprios" (não verificado). Se o próprio app de finanças pessoais conta como "agregação de dados" para o adicional também não foi verificado.

- **Prazo.**
  - Não achei prazo máximo vigente para o BCB analisar o pedido. A Circular 3.875/2018 dava 12 meses e foi revogada pela Circular 3.980/2020 (não verificado se há substituto).
  - A Belvo, que é parte interessada, diz que a licença própria leva "geralmente de um a dois anos" [S](https://belvo.com/pt-br/blog/licenca-open-finance-quanto-custa-quando-vale-fintechs/).
- **Obrigações contínuas** (RC 1/2020, com a redação da RC 21) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21):
  - diretor responsável (art. 32);
  - relatório assinado por ele, submetido a compliance, auditoria interna e risco (art. 33);
  - conselho e diretoria "envolver-se ativamente" (art. 48-A);
  - registros por 5 anos (art. 49);
  - adesão à convenção e registro no Diretório.
  - Prevenção à lavagem de dinheiro: Circular 3.978/2020. Cibersegurança: Res. BCB 85/2021, alterada pela Res. BCB 538/2025.
- **Técnico.**
  - Certificados ICP-Brasil (Manual de Segurança, IN BCB 720/2026).
  - Certificação de segurança pela OpenID Foundation: "As certificações são obrigatórias para todos os participantes entrarem em produção"; recomenda-se pedir "com, pelo menos, 2 meses de antecedência" [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/280297514).
  - Taxa da OpenID Foundation: "$1000.00 USD per new FAPI deployment" para membros e "$5000.00 USD per new FAPI deployment" para não membros [P](https://openid.net/certification/fees/).
- **Governança do Open Finance.** A contribuição mensal é um percentual da despesa total da Estrutura de Governança, por faixa de patrimônio líquido; a menor faixa paga 0,0010% (IN BCB 485/2024) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Instru%C3%A7%C3%A3o%20Normativa%20BCB&numero=485). O valor em reais não foi verificado.

### 2.3 (b) Agregador regulado: como o modelo é enquadrado

- **Antes de 22/10/2026.** A RC 1/2020 já admitia parceria com "entidades não autorizadas" (art. 36), mas proibia incluir no contrato "o compartilhamento de dados de transações de clientes… relativos a produtos e serviços contratados em outras instituições" (art. 38, § 1º, II) [P](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf).
- **Com a RC 21/2026** [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21):
  - esse inciso foi revogado (art. 2º, IV, "a");
  - os papéis passaram a ter nome: "instituição integradora" é a receptora que repassa os dados, e "entidade parceira" é quem os recebe (art. 2º, XVI e XVII);
  - conta como parceria "qualquer contrato" que preveja o compartilhamento de dados de cadastro ou de transações obtidos no Open Finance, ou de dados derivados que identifiquem o cliente (art. 36);
  - a regra não se aplica a quem atua "exclusivamente como operador" no sentido da LGPD (art. 36, § 11).
  - As obrigações do app estão na seção 5.1.
- **O que a imprensa dizia antes.** Notícias de 2026 falavam em proibir dados de transações para não regulados [S](https://www.letsmoney.com.br/noticias/bc-proibir-venda-dados-open-finance/). O texto final não fez isso.
  - As parcerias respondem por "mais de 20% dos mais de 180 milhões de consentimentos" [S](https://finsidersbrasil.com.br/?p=371500).
- **Como o fornecedor descreve.** A Belvo chama o modelo de "licenciamento proxy": "esse consentimento é gerenciado pela Belvo como participante regulado, mas os dados são entregues à empresa cliente". Acrescenta que isso "não isenta a empresa de suas obrigações como controladora de dados" [P, blog do fornecedor](https://belvo.com/pt-br/blog/licenciamento-proxy-open-finance-como-funciona/).

### 2.4 (c) Screen scraping: situação em 2026

- **Norma.** Uma busca por "scraping" e "screen scraping" na base de normas do BCB não trouxe resultados. Não encontramos proibição expressa para empresas fora do Open Finance.
  - Dentro do Open Finance, as credenciais de autenticação ficam fora do que é compartilhado (RC 1/2020, art. 5º, § 4º), e as instituições devem garantir que as demais "não tenham acesso às credenciais" (art. 40, § 2º, III) [P](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf).
- **Parceiros.** A RC 21 obriga o contrato de parceria a proibir que o app adote "medidas com vistas à realização de engenharia reversa ou de procedimentos equivalentes voltados à reconstrução de dados transacionais" (art. 38, XVII) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21).
- **Termos dos bancos.** Exemplo do Nubank: o cliente deve "memorizar as suas senhas para impedir que outras pessoas o utilizem", e transação autorizada por senha é "de sua exclusiva responsabilidade" [P](https://nubank.com.br/contratos/termos-e-condicoes-conta-nubank).
- **Pluggy.** O changelog diz [P](https://docs.pluggy.ai/pt/changelog):
  - "Deprecação dos conectores diretos para pessoa física" (julho/2026);
  - "Bradesco PF, Sicredi PJ e Itaú PF já foram deslistados e o XP segue o mesmo caminho… Não há migração automática".
  - O status público ainda lista conectores diretos ativos, como Inter PF, Caixa PF, Mercado Pago PF e PJ de vários bancos [P](https://status.pluggy.ai/api/status).
- **Belvo.** A lista pública de instituições no Brasil tem 48 entradas, todas via Open Finance (códigos `of…`) [P](https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-institutions).
  - A política de privacidade ainda atribui à Belvo Tecnologia a "coleta e processamento de dados (Scrapping)" [P](https://belvo.com/pt-br/politica-de-privacidade/).
- **ANPD.** A agenda regulatória da ANPD tem o item "Agregadores de dados pessoais", que cita "raspagem de dados" (Res. CD/ANPD 31/2025) [P](https://www.in.gov.br/web/dou/-/resolucao-cd/anpd-n-31-de-22-de-dezembro-de-2025-677950080).
- **Quem ainda oferece.** O Banco MCP (uso pessoal) anuncia "Bancos digitais não-OFB também via credenciais" [P](https://banco.mcp.ai/).
- **Posição de Febraban e bancos** sobre bloqueio técnico de scraping: (não verificado).

### 2.5 (d) Importação de arquivo: o que os 12 bancos exportam

Legenda: **P** = central oficial lida; **P\*** = página oficial lida só pelo trecho de busca (bloqueio 403); **S** = fonte secundária.

| Banco | Conta | Cartão (fatura) | Fontes |
|---|---|---|---|
| Nubank | PDF, OFX e CSV por e-mail, com dados até o dia anterior (P) | PDF pelo app (S) | [Nubank perguntas](https://nubank.com.br/perguntas); [Tecnoblog](https://tecnoblog.net/responde/como-obter-uma-versao-impressa-da-fatura-do-cartao-nubank/) |
| Itaú | Internet banking: "Salvar como", escolhendo o formato (P\*); "OFX / Money 2000" (S) | 2ª via em PDF (S) | [Itaú](https://www.itau.com.br/atendimento-itau/para-voce/conta-corrente/como-consultar-o-meu-extrato-itau-e-o-que-significam-as-siglas-do-extrato); [Meu Contador Online](https://www.meucontadoronline.com.br/blog/como-exportar-o-extrato-do-itau-em-arquivo-ofx/) |
| Bradesco | PF: "OFX (Money 2000 em diante)" (S); Net Empresa (PJ): TXT, XLS, CSV, QIF, OFX, PDF e outros (P) | (não verificado) | [S](https://www.meucontadoronline.com.br/blog/como-exportar-o-extrato-do-bradesco-em-arquivo-ofx/); [simulador Net Empresa](https://www.institucional.bradesco.com.br/simulador_bradesco_net_empresa/conteudo/perfil_corporate/extrato/conta-poupanca_extrato-mensal-periodo2b.asp) |
| Banco do Brasil | "Money 2000+ (ofx)" no internet banking (S) | (não verificado) | [S](https://www.meucontadoronline.com.br/blog/como-exportar-o-extrato-do-banco-do-brasil-em-arquivo-ofx/) |
| Caixa | OFX em "Gerar Arquivo para Gerenciadores Financeiros" (S; o guia trata de conta de empresa) | (não verificado) | [S](https://www.meucontadoronline.com.br/blog/como-exportar-o-extrato-ofx-da-caixa-economica/) |
| Santander | "Extrato (Money)" > "Money 2000 ou superior" (S) | (não verificado) | [S](https://www.meucontadoronline.com.br/blog/como-exportar-o-extrato-do-santander-em-arquivo-ofx/) |
| Inter | PDF, CSV e OFX (por e-mail no app; download no internet banking), com até 2 anos entre data inicial e final (P) | "Exportar fatura" em PDF/CSV, só fatura fechada; a mesma página também diz que a 2ª via "só pode ser emitida em PDF" (P) | [extrato](https://ajuda.inter.co/conta-digital-pessoa-fisica-e-mei/como-acessar-o-extrato-da-conta-digital-pf-kids-ou-you); [fatura](https://ajuda.inter.co/cartao/como-acessar-baixar-ou-emitir-a-segunda-via-da-fatura) |
| C6 | PDF, até 180 dias, por e-mail com senha (P\*) | PDF (P\*) | [extrato](https://www.c6bank.com.br/blog/como-receber-o-extrato-da-sua-conta-do-c6-bank-por-e-mail); [fatura](https://www.c6bank.com.br/blog/como-receber-a-fatura-do-meu-cartao-do-c6-bank-em-pdf) |
| BTG | PDF ou Excel; períodos de 30, 60 ou 90 dias (P\*) | Só fatura fechada; formatos não listados (P\*) | [extrato](https://banking.btgpactual.com/ajuda/contacorrente/extrato); [fatura](https://banking.btgpactual.com/ajuda/cartao/fatura) |
| XP | (não verificado) | (não verificado) | Nenhuma central pública encontrada |
| Mercado Pago | .pdf, .xlsx ou .csv, sem menção a OFX (P\*) | (não verificado) | [Mercado Pago](https://www.mercadopago.com.br/blog/como-obter-extrato-mercado-pago) |
| PicPay | PDF ou CSV; o app mostra os últimos 18 meses (P) | 2ª via de faturas fechadas por e-mail; formato (não verificado) (P) | [extrato](https://meajuda.picpay.com/hc/pt-br/articles/4408499088525); [faturas](https://meajuda.picpay.com/hc/pt-br/articles/22196611297171) |

**Leitura da tabela**
- OFX de conta em fonte primária: só Nubank e Inter (e Bradesco no canal de empresas).
- Sem OFX pelas fontes oficiais: PicPay, Mercado Pago, BTG e C6.
- Fatura de cartão em OFX: nenhuma evidência nos 12 bancos. CSV de fatura só no Inter.

**Problemas citados por concorrentes**
- A Mobills cita OFX sem sinal negativo nas despesas, a não importação de transações com mais de 6 meses e a recusa de OFX que mistura conta e cartão [P](https://mobills.zendesk.com/hc/pt-br/articles/360052620713).
- O Organizze importa OFX só em contas e cartões manuais [P](https://ajuda.organizze.com.br/hc/pt-br/articles/5994104693395).

### 2.6 (e) Notificações e SMS: o que as plataformas permitem

**Android — leitura de notificações**
- **Mecanismo.** O app declara um `NotificationListenerService` com `BIND_NOTIFICATION_LISTENER_SERVICE` [P](https://developer.android.com/reference/android/service/notification/NotificationListenerService), e o usuário concede o acesso numa tela do sistema (`ACTION_NOTIFICATION_LISTENER_SETTINGS`) [P](https://developer.android.com/reference/android/provider/Settings).
- **Google Play.**
  - A política de permissões sensíveis não lista o acesso a notificações, e não encontramos formulário específico para ele [P](https://support.google.com/googleplay/android-developer/answer/9888170).
  - A política de Dados do Usuário trata dados financeiros como sensíveis e exige divulgação em destaque dentro do app, com ação afirmativa do usuário, além da seção Data safety [P](https://support.google.com/googleplay/android-developer/answer/10144311).
  - Se a leitura de notificações exige essa divulgação é interpretação nossa.
- **Android 15.** Em notificações em que o sistema detecta código de verificação (OTP), o conteúdo é ocultado para apps "não confiáveis". A permissão que libera esse conteúdo (`RECEIVE_SENSITIVE_NOTIFICATIONS`) é só para apps de sistema ou pré-instalados [P](https://developer.android.com/about/versions/15/behavior-changes-all). Notificação de compra sem OTP não seria afetada (inferência).
- **Fora da Play Store.**
  - A verificação de desenvolvedor do Android começou em 30/09/2026 para usuários no Brasil (e na Indonésia, em Singapura e na Tailândia) que instalam apps de "participating stores" em aparelhos certificados com Android 7+. O Google Play registra automaticamente "99% of apps" [P](https://developer.android.com/developer-verification).
  - As "configurações restritas" do Android 13+ podem bloquear o acesso a notificações para apps instalados fora da loja [S](https://chromeunboxed.com/android-13-restricted-setting-feature-blocks-sideloaded-apps-from-notifications/).
- **Quem já usa.**
  - Organizze: "exclusivo para o app Android"; usa o "Acesso às notificações" e lê o SMS pela notificação. Economia de bateria e mudanças no texto das mensagens dos bancos quebram a captura [P](https://ajuda.organizze.com.br/hc/pt-br/articles/6373504617363) [P](https://ajuda.organizze.com.br/hc/pt-br/articles/360049302233).
  - Mobills: "Leitura de notificação (Beta)" no Android; depende do "modelo de notificação da sua instituição" [P](https://mobills.zendesk.com/hc/pt-br/articles/360051695074) [P](https://mobills.zendesk.com/hc/pt-br/articles/360052472853).

**Android — permissão de SMS**
- Regra geral: só app "registered as the default SMS, Phone, or Assistant handler".
- Exceção: "Google Play may provide a temporary exception… when… there's currently no alternative method to provide the core functionality". A tabela de exceções inclui "SMS-based money management — For example, apps that track and manage budget" (READ_SMS, RECEIVE_SMS…), "Subject to Google Play review and approval" [P](https://support.google.com/googleplay/android-developer/answer/10208820).
- Se o Google concederia a exceção, já que existe a alternativa de ler notificações: (não verificado).

**iOS**
- **Isolamento.** "All third-party apps are sandboxed… to prevent apps from gathering or modifying information stored by other apps" [P](https://support.apple.com/guide/security/security-of-runtime-process-sec15bfe098e/web).
  - A API de notificações devolve só as notificações do próprio app [P](https://developer.apple.com/documentation/usernotifications/unusernotificationcenter/getdeliverednotifications(completionhandler:)).
- **Filtro de SMS.** A extensão de filtro de mensagens atua só sobre remetentes desconhecidos e serve para barrar mensagens indesejadas, sem gravar dados compartilhados com o app [P](https://developer.apple.com/documentation/identitylookup/sms-and-mms-message-filtering).
- **App Atalhos.**
  - Gatilho "Mensagem" (por remetente e "Message Contains") [P](https://support.apple.com/guide/shortcuts/apdd711f9dff/ios).
  - Gatilho "Transação", baseado em transações da Wallet, "When I tap" [P](https://support.apple.com/guide/shortcuts/transaction-trigger-apd65c67538a/ios).
  - Essas automações podem rodar sem confirmação [P](https://support.apple.com/guide/shortcuts/create-a-new-personal-automation-apdfbdbd7123/ios).
  - O app exporia uma ação "registrar lançamento" via App Intents [P](https://developer.apple.com/documentation/appintents).
  - Se funciona bem com SMS de bancos brasileiros e com cartões brasileiros na Wallet: (não verificado).
- **FinanceKit.** Só EUA (Apple Card, Apple Cash, Savings) e Reino Unido (via open banking); o Brasil não é mencionado [P](https://developer.apple.com/financekit/).
- **O que dizem os concorrentes.**
  - Organizze: "devido a política de privacidade da Apple não conseguimos desenvolver essa feature para iOS" [P](https://ajuda.organizze.com.br/hc/pt-br/articles/360049302233).
  - Mobills: no iOS, o usuário copia o SMS e usa "Importar SMS" [P](https://mobills.zendesk.com/hc/pt-br/articles/360051695074).

---

## 3. Agregadores

### 3.1 Comparativo: status, preço e contrato

| Provedor | Status regulatório | Modelo de cobrança | Preço publicado | Grátis / sandbox | Requisitos de contrato | Fontes |
|---|---|---|---|---|---|---|
| **Pluggy** | Pluggy Brasil Instituição de Pagamento Ltda (CNPJ 37.943.755/0001-30), ITP; Diretório com DADOS e PAGTO desde 04/07/2024 | Mínimo mensal por produto com volume incluso; acima disso, "o excedente é cobrado por requisição" ("Cada chamada de leitura à API (saldo, transação, investimento) conta como uma requisição") | **Dados: "A partir de R$ 2.500 /mês com até 500 conexões ativas"**; Pagamentos: a partir de R$ 500/mês; preço por requisição não publicado | Teste de 15 dias em produção, sem cartão; sandbox sem limite de tempo (ambiente de desenvolvimento limitado a 100 items); MeuPluggy grátis só para uso pessoal | Produção passa por due diligence de compliance com envio de documentos; CNPJ não explícito (não verificado) | [preços](https://www.pluggy.ai/precos), [FAQ](https://docs.pluggy.ai/en/docs/get-started/faq), [changelog](https://docs.pluggy.ai/pt/changelog), [Diretório](https://data.directory.openbankingbrasil.org.br/participants) |
| **Belvo** | Belvo Instituição de Pagamento Ltda (CNPJ 43.215.815/0001-09), "autorizada a atuar como agregador de dados no âmbito do Open Finance"; Diretório com DADOS e PAGTO desde 06/10/2022 | Mensalidade + uso; atualizações históricas, investimentos e enriquecimento são cobrados à parte, sem tabela pública | **Launch: R$ 1.500/mês**, "exclusivo para instituições de crédito, bancos, pagamentos e fintechs, proptech e mercado imobiliário e telecom. Demais segmentos e uso pessoal não são atendidos"; Growth sob consulta | Sandbox grátis só "para fins de teste e avaliação"; programa para startups com R$ 25 mil em créditos (até 250 clientes, sediadas no Brasil, por seleção, "por tempo determinado") | Contrato de 12 meses com renovação automática, aviso de 60 dias e reajuste por IPCA; cliente = "empresa" | [planos BR](https://belvo.com/pt-br/planos-precos/), [startups](https://belvo.com/pt-br/startups/), [termos](https://belvo.com/pt-br/terms-service/), [privacidade](https://belvo.com/pt-br/politica-de-privacidade/) |
| **Klavi** | Klavi Instituição de Pagamento e Gestão de Dados (CNPJ 44.459.799/0001-54); Diretório com DADOS e PAGTO desde 02/02/2023 | Não publicado | — | Ambientes sandbox, testing e produção | Credenciais de produção "through Klavi business personnel… or Klavi console" | [docs](https://docs.klavi.ai/connect/lqUr-overview), [site](https://www.klavi.ai/) |
| **Lina Open X** (Data Link) | Lina Instituição de Pagamento S.A., modalidade ITP | Não publicado ("agende uma demo") | — | "Sandbox disponível"; teste de até 10 dias, a critério da Lina | — | [Data Link](https://linaopenx.com.br/data-link), [termos v4](https://consentimento.linaopenx.com.br/docs/termos-e-condicoes.pdf) |
| **Iniciador** (Dados Bancários) | IP autorizada; papel de dados no Diretório | Não publicado ("fale com o comercial") | — | Documentação exige login | Cliente "Pessoa jurídica… responsável… por obter os consentimentos" | [produto](https://iniciador.com.br/produtos/dados-bancarios), [termos](https://iniciador.com.br/legal) |
| **Celcoin** (Financial Data) | IP; papéis DADOS, CONTA e PAGTO | Não publicado | — | Documentação pública | O produto "Receptor de dados" exige que o cliente seja instituição autorizada; o Financial Data é sob consulta | [Financial Data](https://www.celcoin.com.br/open-finance/financial-data/), [receptor](https://developers.celcoin.com.br/docs/receptor-de-dados) |
| **Cumbuca** | IP (ITP); "proxy" das APIs oficiais sob a licença dela | "Cobramos somente por pagamentos iniciados com sucesso e consentimentos utilizados. Consentimentos parados… não são cobrados"; sem tabela | — | Sandbox; MCP grátis para os próprios dados | "sua marca é sempre exibida no consentimento" | [site](https://www.cumbuca.com/) |
| **Polp** (revenda, **não regulada**) | Polp Tecnologia; usa a Celcoin e, só para contas criadas antes de 16/08/2026, a Pluggy | Plano mensal + valor por consentimento ativo/mês, pós-pago | **Starter R$ 159, Pro R$ 299, Ultra R$ 499 por mês**; consentimento via Celcoin: R$ 3,60 (0–100), R$ 3,10 (100–1.000), R$ 2,70 (1.000–5.000), R$ 2,30 (5.000–10.000), R$ 2,00 (10.000+) | Sandbox "nunca é cobrado"; consentimentos em WAITING_USER_INPUT, ERROR ou OUTDATED não são cobrados | "pessoas jurídicas regularmente constituídas (CNPJ ativo)"; a Polp é operadora e o cliente é controlador | [planos (API pública)](https://api.polp.com.br/api/v1/billing/plans), [faixas](https://api.polp.com.br/api/v1/billing/tiers), [termos](https://polp.com.br/termos), [FAQ](https://polp.com.br/) |

**Outros provedores do levantamento**
- **TecnoSpeed (API Extrato Open Finance).** Diz ser "credenciada pelo BCB", mas não aparece no Diretório; foco em ERPs. Um dev relatou "R$1.500 de adesão + R$540/mês" [S](https://www.tabnews.com.br/GuilhermeVieira/estou-desenvolvendo-um-app-de-financas-pessoais-e-nao-consigo-pagar-o-open-finance-pluggy-r2-5k-mes-belvo-r6k-mes-tecnospeed-r1-5k-de-entrada-r540). Credenciamento e parceiro regulado: (não verificado).
- **Banco MCP.** Produto de uso pessoal: R$ 0 a R$ 49,90/mês, "Agregação via Pluggy" [P](https://banco.mcp.ai/). Os termos proíbem "revenda indevida de acesso" [P](https://banco.mcp.ai/termos).
- **Sem produto de dados para terceiros ou inativos:**
  - Quanto: site fora do ar; quan.to dá 404 desde mai/2023.
  - Teddy: marketplace de crédito.
  - TecBan, Sensedia, Ozone e Dock: tecnologia para instituições.
  - Prometeo: sem evidência de dados de conta e cartão no Brasil.
- **Estrangeiros que não atendem o Brasil:**
  - Plaid: lista de países sem BR [P](https://plaid.com/docs/api/link/);
  - Yapily e TrueLayer/Tink: foco europeu;
  - Fintoc: Chile e México [P](https://docs.fintoc.com/docs/welcome);
  - Syncfy: México e outros, sem Brasil [P](https://syncfy.com/en/coverage).

**Revendas e a RC 21.** A RC 21 obriga o contrato a proibir a entidade parceira de "estabelecer modelo de negócio, para o compartilhamento, com terceiros, de dados obtidos por meio do contrato de parceria" (art. 38, XIII). A Polp se declara "operadora de dados em nome do Cliente" [P](https://polp.com.br/termos), e operadores ficam fora da regra de parceria (art. 36, § 11). Como revendas vão se enquadrar: (não verificado).

### 3.2 Cobertura dos 12 bancos

PF = pessoa física; PJ = pessoa jurídica; OF = via Open Finance; direto = conector por credenciais (scraping).

| Banco | Pluggy (OF) | Pluggy (direto) | Belvo (OF) | Klavi (OF) |
|---|---|---|---|---|
| Nubank | PF e PJ | — | PF e PJ | PF e PJ |
| Itaú | PF e PJ (Empresas, BBA) | Só PJ | PF (+ Itaucard) e PJ | PF e PJ |
| Bradesco | PF e PJ | Só PJ | PF e PJ (+ Bradescard) | PF e PJ |
| Banco do Brasil | PF e PJ (+ Investimentos BB) | PJ e BB Previdência | PF e PJ | PF e PJ |
| Caixa | PF, PJ, Caixa Tem e clientes sem conta | PF e PJ | PF e PJ | PF e PJ |
| Santander | PF, PJ e Corretora | Só PJ | PF (+ Santander Card) e PJ | PF e PJ |
| Inter | PF e PJ | PF e PJ | **Não listado** | PF e PJ |
| C6 | PF e PJ | — | **Não listado** | PF e PJ |
| BTG | PF, PJ e Investimentos | Investimentos | PF e PJ (+ BTG Empresas) | Banking, Empresas e Investimentos |
| XP | XP Banking PF e PJ | XP Investimentos (em descontinuação) | PF e PJ | PF e PJ |
| Mercado Pago | PF e PJ | PF | PF e PJ | PF e PJ |
| PicPay | PF e PicPay Negócios | — | Só PF | PF e PicPay Negócios |

**Fontes**
- Pluggy: API pública da página de status, gerada em 26/10/2026, com 368 instituições (206 via Open Finance e 162 diretas) [P](https://status.pluggy.ai/api/status). Segundo a Pluggy, via Open Finance os 12 trazem conta, transações, cartão e investimentos; a exceção é o PicPay Negócios, que só traz conta e transações [P](https://docs.pluggy.ai/en/docs/open-finance/institutions-coverage).
- Belvo: lista pública de 48 instituições, sem Inter e sem C6 [P](https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-institutions).
- Klavi: lista "Participant Institutions", atualizada em 17/07/2026 [P](https://docs.klavi.ai/connect/participant-institutions).
- Lina ("+60 instituições") e Iniciador ("100+ instituições") não publicam a lista por banco no material lido.

**Observações**
- Cobertura não é garantia de qualidade (seção 4.5).
- A Pluggy informou que "Santander Cartões (798)" foi "deslistado do Open Finance" em set/2026 [P](https://docs.pluggy.ai/pt/changelog). Se os cartões Santander seguem disponíveis pelo conector Santander principal: (não verificado).

### 3.3 Dados, enriquecimento, webhooks e atualização

| Item | Pluggy | Belvo | Klavi | Polp |
|---|---|---|---|---|
| **Produtos via Open Finance** | Contas, transações, cartões e faturas, identidade, empréstimos, investimentos [P](https://docs.pluggy.ai/docs/open-finance-regulated) | Owners, contas, transações, faturas, saldos, investimentos (custo adicional), empréstimos (`loan_data`) [P](https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-introduction) | Conta, cartão ("Identification, Limits, Invoices, Transactions"), crédito, investimentos, câmbio [P](https://docs.klavi.ai/connect/consent-permissions) | Contas, cartões e faturas, crédito, investimentos, câmbio [P](https://polp.com.br/termos) |
| **Faturas de cartão** | Vencimento, total, mínimo, encargos e pagamentos [P](https://docs.pluggy.ai/en/docs/products/credit-card-bills) | Só fechadas: total, mínimo, vencimento, `is_installment`, encargos e pagamentos | "Invoices" | "contas, cartões e faturas" |
| **Campos de parcela** | Número da parcela, total de parcelas, data da compra, `billId`; desde jul/2026, `billForecastDate` e `billClosingDate` [P](https://docs.pluggy.ai/pt/changelog). O valor total da compra parcelada é "direct connectors only" [P](https://docs.pluggy.ai/en/docs/connections/open-finance-vs-direct-fields) | `installment_identifier` ("according to the institution") e `number_of_installments`; dados de fatura "only returned for 'closed' bills" | (não verificado) | "Parcelas automáticas", "agrupamento de parcelas" [P](https://api.polp.com.br/api/v1/billing/plans) |
| **Parcelas futuras** | Dependem do banco (seção 4.5) | Sem recurso ou campo para parcelas futuras na especificação | (não verificado) | (não verificado) |
| **Categorização / enriquecimento** | Categorizador com IA, regras por cliente, merchant com CNPJ/CNAE, "Same person transfer", "Credit card payment", API de recorrência; recurso pago ("opt-in premium feature") [P](https://docs.pluggy.ai/en/docs/products/transaction-categorization) | Sob pedido: 16 categorias e 77 subcategorias, merchant; Incomes, Recurring Expenses e Risk Insights com custo adicional [P](https://developers.belvo.com/developer_resources/enrichment-overview) | API de categorização (klaaS) [P](https://docs.klavi.ai/klaas/transactions-categorization) | Categorização, recorrências e normalização de estabelecimentos; usa a OpenAI (EUA) como subprocessador [P](https://polp.com.br/subprocessadores) |
| **Webhooks** | `item/*`, `connector/status_updated`, `transactions/created`, `updated` e `deleted` [P](https://docs.pluggy.ai/en/docs/developer-tools/webhooks-ref) | `historical_update`, `new_*_available`, `transactions_updated` e `deleted`, aviso de consentimento a expirar ou expirado, `operational_limits_reached` [P](https://developers.belvo.com/developer_resources/resources-webhooks-aggregation) | Assinados com HMAC SHA-256, com reenvio [P](https://docs.klavi.ai/connect/lqUr-overview) | Sim |
| **Atualização automática** | "every 24, 12, or 8 hours based on your subscription" (recurso premium, só em produção) [P](https://docs.pluggy.ai/en/docs/connections/item); "Batch process are prohibited" [P](https://docs.pluggy.ai/en/docs/integration-checklist/data-sync) | Links recorrentes: padrão "once every seven days"; 6 h, 12 h, 24 h ou 30 dias mudando o preço [P](https://developers.belvo.com/developer_resources/resources-links-overview) | (não verificado) | Horários por recurso "em breve" no plano Pro |
| **Histórico inicial** | Até 12 meses; depois, olha os últimos 7 dias [P](https://docs.pluggy.ai/en/docs/products/credit-card-installments) | "the last 12 months of Account, Owner, Transaction, and Credit Card Bill information" [P](https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-introduction) | (não verificado) | (não verificado) |
| **Consentimento padrão** | "By default, we request non-expiring consents" (Inter PJ: 1 ano) [P](https://docs.pluggy.ai/en/docs/connections/consents) | 3, 6, 9 ou 12 meses, ou "Indeterminado"; sem valor configurado, "Indeterminado" vem pré-selecionado [P](https://developers.belvo.com/products/aggregation_brazil/ofda-widget-branding-and-customization) | "Undetermined: valid until the user revokes it" ou 12 meses [P](https://docs.klavi.ai/connect/concepts) | (não verificado) |

### 3.4 Detalhes por provedor

**Pluggy**
- **MeuPluggy.** É uma carteira pessoal em meu.pluggy.ai, lida via "Conector 200" [P](https://meu.pluggy.ai/api-guide).
  - "Gratuito para uso pessoal", "até 5 conexões ativas, todas de contas do mesmo titular".
  - "Posso usar essa API para fins comerciais? Não. Uso comercial exige o plano pago" [P](https://www.pluggy.ai/meu-pluggy).
  - O FAQ de preços admite que "Tecnicamente sim" dá para usar o conector de graça, mas lista limites para produto real [P](https://www.pluggy.ai/precos).
- **Histórico de preços** (snapshots do próprio site):

  | Data | Plano e preço |
  |---|---|
  | jun/2023 | Pro "a partir de R$100,00" ([Wayback](https://web.archive.org/web/20230605170413/https://pluggy.ai/pricing)) |
  | mai/2024 | "a partir de R$500,00" ([Wayback](https://web.archive.org/web/20240528195836/https://www.pluggy.ai/pricing)) |
  | jan/2025 | "Plano Básico A partir de R$2.500/mês" com conexões "ilimitadas" ([Wayback](https://web.archive.org/web/20250119093813/https://www.pluggy.ai/pricing)) |
  | out/2026 | "até 500 conexões ativas" ([atual](https://www.pluggy.ai/precos)) |

- **Ciclo de fatura.** Perto do fechamento, transações de cartão podem ser apagadas e recriadas com outro id. O identificador estável é o id do banco, que só existe via Open Finance [P](https://docs.pluggy.ai/en/docs/products/credit-card-statement-cycle).
- **Incidentes.** O status público lista 171 incidentes e 75 manutenções entre 27/05 e 24/10/2026. Em 26/10/2026 havia 10 incidentes de dados abertos [P](https://status.pluggy.ai/api/status):
  - Inter, compras parceladas "retornadas incorretamente" (desde 23/10/2026);
  - XP Banking, compras parceladas não retornadas (desde 23/07/2026);
  - PicPay, transações de cartão não retornadas (desde 02/10/2026);
  - Bradesco, transações de conta corrente não retornadas (desde 22/07/2026);
  - Itaú Empresas, transações ausentes e datas divergentes (desde 20/10/2026).
- **Alegação do fornecedor:** "+500 empresas com contrato ativo". Entre os clientes citados há um app de finanças pessoais, o Pierre [P](https://www.pluggy.ai/meu-pluggy).

**Belvo**
- **Histórico de preços (página BR):**

  | Data | Preço |
  |---|---|
  | jun/2022 | "R$ 600 /mês" ([Wayback](http://web.archive.org/web/20220630220421/https://belvo.com/pt-br/planos-precos/)) |
  | out/2024 a nov/2025 | "R$ 6000/ mês" ([Wayback](http://web.archive.org/web/20251102121036/https://belvo.com/pt-br/planos-precos/)) |
  | hoje | R$ 1.500/mês |

  A página global mostra Launch a "1,000 USD/ month" [P](https://belvo.com/plans-and-pricing/).
- **Atraso e status.** As transações têm status PENDING ou PROCESSED. O help center diz que as instituições "take multiple days to process a transaction… finalized data after 2 days or occasionally even longer" [P](https://support.belvo.com/hc/en-us/articles/22296314310941).
- **Gestão de consentimento.** O cliente precisa oferecer ao usuário acesso fácil à gestão de consentimentos (Meu Portal Belvo) e tratar o webhook de consentimento expirado [P](https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-mybelvoportal).
- **Alegação do fornecedor:** "+26 milhões De consentimentos ativos recebidos pela Belvo" [P](https://belvo.com/pt-br/).

**Klavi**
- Pede dois consentimentos: o do Open Finance e um consentimento LGPD, "authorisation granted by the user to Klavi to allow data sharing with its partners" [P](https://docs.klavi.ai/connect/quick-start).
- O changelog tem entrada de 21/09/2026; Inter, C6 e PagBank entraram em 22/07/2025 [P](https://docs.klavi.ai/connect/release-history).

**Iniciador**
- "12 meses" de histórico, "~2 dias" de integração técnica e "~1 semana até produção" [P](https://iniciador.com.br/produtos/dados-bancarios).
- Também vende receptor *white-label* para instituições reguladas [P](https://iniciador.com.br/en/infraestrutura/receptora-de-dados).

**Cumbuca**
- "Integre… sem a necessidade de obter licenças e capital regulatório".
- "Quando sua empresa tiver a própria licença, não há migração" [P](https://www.cumbuca.com/).

**Polp**
- FAQ: "Estou desenvolvendo um aplicativo de finanças pessoais. Posso usar a Polp? Sim" [P](https://polp.com.br/).
- O parceiro regulado (Celcoin) impõe regras à jornada: textos obrigatórios, área de gestão de consentimentos no primeiro nível do menu e revogação sem depender de suporte [P](https://polp.com.br/docs/celcoin/termos).

### 3.5 Relatos de mercado (secundários, não são cotação)

- **TabNews, 21/05/2026.** Um dev de app de finanças pessoais relatou "Pluggy ~R$2.500/mês, Belvo ~R$6.000/mês, Tecnospeed R$1.500 de adesão + R$540/mês". Outro dev disse que tentou negociar com a Pluggy e "o piso é esses ~R$2,5k" [S](https://www.tabnews.com.br/GuilhermeVieira/estou-desenvolvendo-um-app-de-financas-pessoais-e-nao-consigo-pagar-o-open-finance-pluggy-r2-5k-mes-belvo-r6k-mes-tecnospeed-r1-5k-de-entrada-r540).
- **TabNews, 17/06/2026** [S](https://www.tabnews.com.br/GuilhermeVieira/resolvi-o-problema-do-open-finance-caro-que-postei-aqui-achei-uma-alternativa-e-ja-esta-em-producao):
  - o mesmo dev adotou o Banco MCP;
  - outro usuário paga R$ 159 à Polp e diz ter negociado "5 reais o consentimento".

---

## 4. Regras do Open Finance que afetam o produto

### 4.1 Quem recebe os dados

- Só instituição autorizada é receptora (seção 2.2). Um app não regulado só recebe dados do Open Finance como "entidade parceira" de uma "instituição integradora" (RC 21) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21).
- O agregador "deve assegurar… a identificação clara da entidade parceira durante o processo de obtenção e de gestão do consentimento" (art. 38-A). Ele também responde pela "confiabilidade… segurança… e pelo sigilo do compartilhamento e dos dados compartilhados" (art. 39).
- Que nome o usuário vê na tela de consentimento dentro do app do banco (app, agregador ou ambos): (não verificado).
  - No widget da Belvo, com branding configurado, aparece "[company_name] uses Belvo to connect your account" [P](https://developers.belvo.com/products/aggregation_brazil/ofda-widget-branding-and-customization).

### 4.2 Consentimento: prazo, renovação e revogação

- **Prazo.**
  - A RC 7/2023 trocou o "limitado a doze meses" por "ter prazo de validade compatível com as finalidades" (RC 1/2020, art. 10, § 1º, III) [P](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf).
  - Na API, o campo de expiração não é enviado "em casos de consentimento com prazo indeterminado" [P](https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/consents/3.3.1.yml).
  - A transmissora recusa "Data de expiração maior que um ano, diferente de prazo indeterminado". Na criação, portanto, vale até 12 meses ou indeterminado [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/219480491).
  - O guia de experiência pede prazo pré-preenchido e pelo menos mais uma opção [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/2172421135).
- **Mudar condições sem passar pelo banco.**
  - Para alterar finalidade, prazo ou dados, a receptora informa o cliente e obtém concordância "específica e ativa", "ficando dispensada a obrigatoriedade dos procedimentos de autenticação do cliente e confirmação no ambiente da instituição transmissora" (art. 10, §§ 7º e 8º, redação da RC 10/2024) [P](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf).
  - Trocar de banco transmissor exige consentimento novo (§ 2º).
- **Renovação na API** [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/219480491):
  - é feita "via backend, sem redirecionamento" (`POST /consents/{id}/extends`);
  - "Somente consentimentos ativos, no status AUTHORISED, podem ser renovados";
  - consentimento expirado vira REJECTED, que é estado final, e exige um consentimento novo pelo banco;
  - não vale para empresas com várias alçadas de aprovação.
  - Oferecer renovação é opcional para a receptora [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/2172422374).
- **Fluxo desacoplado.** A RC 21 criou o "fluxo de aprovação desacoplado": o pedido é feito na receptora e aprovado depois no banco, "sem redirecionamento automático", "observado o disposto em regulamentação específica" (art. 2º, XIX; art. 9º, §§ 2º e 3º) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21). Entrada em produção: (não verificado).
- **Revogação.**
  - Pode ser feita "a qualquer tempo", "ao menos pelo mesmo canal… no qual foi concedido", e para dados é "de forma imediata" (art. 15) [P](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf).
  - É "vedado à instituição transmissora… propor ao cliente a revogação" (§ 2º, redação da RC 21).
  - Apagar a conexão no agregador:
    - Pluggy: a exclusão revoga o consentimento e apaga os dados [P](https://docs.pluggy.ai/en/docs/connections/item-lifecycle).
    - Belvo: ao apagar o link, "all data associated with that user will be removed from Belvo and their consent will be automatically revoked". O mesmo parágrafo acrescenta: "However, the original consent remains intact and the user must revoke it themselves using the My Belvo Portal" [P](https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-introduction).
- **Finalidade.** Os dados devem ser "pertinentes às finalidades determinadas" (art. 12). É vedado obter consentimento "por meio de contrato de adesão", com aceite pré-preenchido ou "de forma presumida" (art. 10, § 3º) [P](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf).

### 4.3 Limites de consulta (frequência máxima de atualização)

**Regra** [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/17924220)
- A contagem é feita por "mês", "endpoint", "objeto mais granular referenciado na chamada (consentimento/recurso/produto)", "cliente (CPF ou CNPJ)" e "instituição consumidora da informação".
- Implementar os limites é opcional para o banco, mas se ele implementar "devem garantir o consumo mínimo".
- Só contam respostas 2XX, e páginas adicionais não contam. Quem excede recebe HTTP 423.

**Valores mínimos por endpoint** (página "Referência", versão 26 de 25/06/2026) [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/17957025):

| API | Endpoint | Consultas/mês |
|---|---|---|
| Contas | `/accounts`, `/accounts/{id}` | 8 |
| Contas | `/balances`, `/overdraft-limits` | 420 |
| Contas | `/transactions` (histórico) | 8 |
| Contas | `/transactions-current` | 240 |
| Cartão | `/accounts` | 8 |
| Cartão | `/accounts/{id}` | 120 |
| Cartão | `/bills`, `/bills/{billId}/transactions` | 30 |
| Cartão | `/limits` | 240 |
| Cartão | `/transactions` (histórico) | 8 |
| Cartão | `/transactions-current` | 240 |

**O que isso significa na prática**
- 240 por mês dá ~8 consultas por dia de transações recentes por conta ou cartão; 30 por mês dá ~1 por dia de faturas (cálculo nosso).
- A doc da Pluggy ainda mostra 4 por mês para histórico e listagem, abaixo do mínimo oficial atual de 8 [P](https://docs.pluggy.ai/en/docs/open-finance/rate-limits).
- Como a contagem é por instituição consumidora, todos os apps atendidos pelo mesmo agregador dividem a cota de um mesmo CPF no mesmo banco (inferência). A Pluggy avisa que o limite pode já estar parcialmente usado se o usuário compartilhou com outra plataforma no mês [P](https://docs.pluggy.ai/en/docs/open-finance/rate-limits).
- O Organizze atualiza 1 vez por dia, com +2 buscas manuais (Conectado) ou +4 (Plus) [P](https://ajuda.organizze.com.br/hc/pt-br/articles/47028882431891).

### 4.4 Atraso e histórico

- **Atraso.** A transmissora informa data e hora da última atualização, e a defasagem máxima admitida é "até cinco minutos, com relação a dados relativos ao saldo e às transações realizadas em conta de depósitos ou de pagamento" e "até uma hora, para os demais casos" (Res. BCB 32, art. 6º-A) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=32). Se o cartão entra na regra dos 5 minutos: (não verificado).
- **Cartão.** Só transações "após conciliado" [P](https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/credit-cards/2.3.1.yml). Compras em processamento não aparecem.
- **Histórico.** O mínimo é de 12 meses (RC 1/2020, art. 5º, § 5º) [P](https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf).
  - Contas: `/transactions` cobre "12 meses no passado mais 12 meses no futuro" [P](https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/accounts/2.4.2.yml).
- **Na prática.**
  - Pluggy: "can take up to 24h".
  - Belvo: "after 2 days or occasionally even longer".
  - Organizze: "até 48h" (fontes nas seções 1.2 e 3.4).

### 4.5 Cartão de crédito e parcelas (destaque)

**O que a API oficial expõe hoje (v2.3.1, estável)** [P](https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/credit-cards/2.3.1.yml)
- **Endpoints:** contas de cartão, limites, faturas (`/bills`), transações por fatura, transações (12 meses) e transações atuais (7 dias).
- **Fatura aberta não vem como fatura.** "Só deve ser informada uma fatura já fechada. Qualquer pagamento deve ser contado para a última fatura fechada."
  - As compras da fatura aberta aparecem em `/transactions` e `/transactions-current`, sem `billId`.
- **Campos de parcela.**
  - `chargeIdentificator`: "Número da parcela que está sendo informada", obrigatório se `paymentType` for `A_PRAZO`.
  - `chargeNumber`: "Quantidade de parcelas", preenchido "quando houverem parcelas relacionadas a transação".
  - `billPostDate`: "Data em que a transação foi inserida na fatura".
  - `payeeMCC`: categoria do estabelecimento.
- **Parcelas futuras.** Não há campo para parcelas ainda não lançadas.
  - O endpoint `/limits` traz limite usado e disponível por linha (inclusive `CREDITO_PARCELADO`). Não encontramos campo separado para "limite comprometido por parcelas futuras".
- **Pagamentos da fatura** aparecem como `VALOR_PAGAMENTO_FATURA_PARCELADO`, `VALOR_PAGAMENTO_FATURA_REALIZADO` ou `OUTRO_VALOR_PAGO_FATURA`.

**O que muda na v2.4.0 (especificação final)** [P](https://raw.githubusercontent.com/OpenBanking-Brasil/all-services-repo/refs/heads/main/api_credit-cards-accounts_-_open_finance_brasil/2.4.0.yaml)
- **`billForecastDate`:** "Mês e ano em que a transação/parcela será faturada, podendo ser uma fatura aberta, futura ou fechada. Caso seja uma transação futura, essa data poderá ser ajustada conforme o cliente altere os ciclos de sua fatura."
- **Valor:** "Para compras parceladas o valor do campo deve corresponder ao valor da parcela".
- **Data da compra:** "Nas compras parceladas, o transactionDateTime deve ser idêntico em todas as parcelas".
- **Novos itens:** tipo de transação `PAGAMENTO_FATURA` e campo `billClosingDate` (data de fechamento) na fatura.
- **Situação:** changelog da API de cartão em [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/1739292717). Data em que a v2.4.0 vira obrigatória e quantos bancos já a expõem: (não verificado).
  - A Pluggy já expõe `billForecastDate` e `billClosingDate` desde jul/2026 [P](https://docs.pluggy.ai/pt/changelog).

**Como os bancos se comportam hoje** (documentação da Pluggy) [P](https://docs.pluggy.ai/en/docs/products/credit-card-installments)
- A regulação exige os dados de compras parceladas, "but it does not specify how that data must be formatted or structured". Por isso os bancos variam: "some post all installments at once, others create them month by month, and others return only the current installment".
- Lançar parcela a parcela, mês a mês, "is the most common behavior among the major banks".
- Não existe campo que agrupe as parcelas da mesma compra. É preciso inferir por `installmentNumber`, `totalInstallments`, data da primeira parcela e nome do estabelecimento.
- Faturas abertas não são retornadas "until the bills are closed or overdue".
- Exceções que a Pluggy documenta:
  - o BTG Investimentos manda as parcelas seguintes como pendentes;
  - desde set/2026, as parcelas futuras de BTG e Mercado Pago chegam com data futura;
  - em set/2026 a Pluggy corrigiu Itaú, Banrisul e Inter, que mandavam o valor total em cada parcela ([changelog](https://docs.pluggy.ai/pt/changelog)).

**O que o Organizze informa aos usuários** (editado em 01/10/2026) [P](https://ajuda.organizze.com.br/hc/pt-br/articles/34229891699475)
- "Grande parte dos bancos não disponibilizam as informações de parcelas futuras de um cartão de crédito via Open Finance."
- "É necessário aguardar o fechamento da fatura para que, enfim, esses lançamentos sejam informados pelo Open Finance."
- "No momento, as instituições que importam as faturas de meses futuros são: Nubank, Itaú e Mercado Pago."
- O Organizze também oferece "mover" lançamentos entre faturas e "esconder" itens "por conta de limitações de algumas instituições bancárias" [P](https://ajuda.organizze.com.br/hc/pt-br/articles/49964437896979).

**Ligação com as queixas do Organizze (inferência).** A queixa "compra parcelada aparece inteira no mês" combina com bancos que mandam o valor total em cada parcela ou só a parcela atual. A queixa "sem projeção de faturas futuras" combina com a ausência de parcelas futuras na v2.3.1. As duas dependem da adoção da v2.4.0 pelos bancos.

### 4.6 Transferências, pagamento de fatura e lançamentos futuros na conta

- **API de contas (v2.4.2)** [P](https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/accounts/2.4.2.yml):
  - `partieCnpjCpf`: CPF ou CNPJ do pagador ou recebedor; "a partir de 02/05/23, o envio das informações de identificação de contraparte tornou-se obrigatória para transações de pagamento";
  - `type`: inclui PIX, TED, `TRANSFERENCIA_MESMA_INSTITUICAO`, BOLETO, CARTAO e outros;
  - `completedAuthorisedPaymentType`: `TRANSACAO_EFETIVADA`, `LANCAMENTO_FUTURO` ou `TRANSACAO_PROCESSANDO`;
  - `/transactions` cobre até 12 meses no futuro.
- **Inferência.** O CPF da contraparte igual ao do titular permite detectar transferência entre contas próprias. É a queixa de "transferência contada como receita e despesa".
- **Pluggy** já categoriza "Same person transfer - PIX/TED" e "Credit card payment" e, desde set/2026, marca pagamento de fatura como `PAGAMENTO_FATURA` [P](https://docs.pluggy.ai/en/docs/products/transaction-categorization).
- **Organizze** depende de o usuário marcar "transferência pessoal" ou "pagamento de fatura" manualmente ([documento anterior](organizze-funcionalidades.md), seção 6.5).

### 4.7 Pessoa jurídica e MEI

- O cliente pode ser "qualquer pessoa natural ou jurídica" (RC 1/2020, art. 2º, II, redação da RC 21) [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21).
- **Regras do consentimento PJ** [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/219480491):
  - leva o CNPJ;
  - PF e PJ não podem estar no mesmo consentimento (erro `PERMISSAO_PF_PJ_EM_CONJUNTO`);
  - a tela só mostra bancos que "Suporta Contas PJ".
- **MEI.** "É vedado a exigência de poderes ou criação de etapas adicionais para aprovação de consentimentos em empresas (clientes PJ) com apenas um sócio pessoa natural (exemplo: MEI, EI, SLU e afins)".
- **Empresas com várias alçadas.** O consentimento fica pendente até todos aprovarem, e a renovação simplificada não vale [P](https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/2172423431).
- A API de cartão "Não possui segregação entre pessoa natural e pessoa jurídica" [P](https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/credit-cards/2.3.1.yml).
- O Organizze só conecta contas PJ no plano Conectado Plus [P](https://ajuda.organizze.com.br/hc/pt-br/articles/34229891699475).

---

## 5. Obrigações (LGPD e contrato)

### 5.1 Obrigações do app como "entidade parceira" (RC 21/2026)

Texto lido no BCB [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21) (via [API](https://www.bcb.gov.br/api/conteudo/app/normativos/exibenormativo?p1=Resolu%C3%A7%C3%A3o%20Conjunta&p2=21)). A norma "entra em vigor na data de sua publicação"; a data no DOU não aparece na base do BCB (não verificado).

| Tema | Regra | Artigo |
|---|---|---|
| Consentimento | O agregador deve "identificar o cliente" e "obter o consentimento do cliente de forma expressa e relativamente a cada entidade parceira", dado "no ambiente da instituição integradora" | Art. 36, §§ 8º e 9º |
| Transparência | Identificação clara do parceiro no consentimento; informar o cliente "que a entidade parceira não atua em nome da instituição integradora" | Art. 38-A; art. 38, § 2º |
| Retenção | Tratamento "apenas o tempo necessário para a finalidade do consentimento", informação ao titular e "eliminação dos dados" ao término | Art. 36, § 7º |
| Verificação prévia do app | Cumprimento da regulação, incluindo "requisitos técnicos e operacionais a serem definidos pelo Banco Central"; confidencialidade, integridade, disponibilidade e recuperação; certificações; relatórios de auditor independente; controles de acesso | Art. 37, II |
| Auditor | A verificação deve ser "atestada por auditor independente registrado na Comissão de Valores Mobiliários", "em periodicidade, no mínimo, anual" | Art. 37, § 5º; art. 38, XX |
| Proibições ao app | Substabelecer o contrato (XII); compartilhar com terceiros os dados recebidos, "inclusive" dados derivados (XIII); engenharia reversa para reconstruir dados transacionais (XVII) | Art. 38 |
| Outras cláusulas obrigatórias | Países e regiões para onde os dados podem ir (III); medidas de segurança (IV); avisar subcontratação (VI); acesso do BCB aos contratos, dados e relatórios do auditor (VII); comunicar incidentes (X e § 3º); tratar demandas de clientes (XI); gestão de consentimento pelo parceiro (XV); encerramento com transparência ao cliente (XIX) | Art. 38 |
| Nome do app | Vedada parceria com entidade que "em sua nomenclatura, empregue termos característicos definidores da nomenclatura das instituições do Sistema Financeiro Nacional ou do Sistema de Pagamentos Brasileiro ou expressões similares em vernáculo ou em idioma estrangeiro". Se a vedação surgir depois, o contrato deve ser encerrado em até 6 meses. | Art. 36, §§ 5º, IV e 5º-A |
| Poder do BCB | Vetar ou restringir o compartilhamento; suspender ou encerrar a parceria | Art. 52 |
| Contratos existentes | Lista de parceiros ao BCB "até 1º de dezembro de 2026"; adequação "dos contratos e dos consentimentos existentes" conforme cronograma do BCB | Art. 52-A |

- **O que é "nomenclatura".** A RC 17/2025 define o termo, para instituições autorizadas, como "o nome empresarial, o nome fantasia, a marca e o domínio de internet" [P](https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=17). Aplicar essa definição ao app parceiro é inferência. A norma não lista os termos vedados.
- **Exceção do operador.** A regra não se aplica a quem "atue exclusivamente como operador" (art. 36, § 11). Um app que decide as finalidades do tratamento tende a ser controlador (inferência; ver 5.2).

### 5.2 LGPD e normas correlatas

**Bases legais e consentimento** — [Lei 13.709/2018](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm) [P]
- Bases possíveis: consentimento (art. 7º, I) ou execução de contrato "a pedido do titular" (art. 7º, V).
- Requisitos do consentimento:
  - "livre, informada e inequívoca" (art. 5º, XII);
  - em "cláusula destacada" (art. 8º, § 1º);
  - o ônus da prova é do controlador (§ 2º);
  - "autorizações genéricas… serão nulas" (§ 4º);
  - revogação "por procedimento gratuito e facilitado" (§ 5º).
- Dado financeiro não está na lista de dados sensíveis (art. 5º, II). Se transações que revelam religião, saúde ou filiação sindical atraem o art. 11, § 1º: (não verificado).
- Qual base é a recomendada para "conectar banco": (não verificado). A ANPD ainda não regulou consentimento; o tema está na agenda (Res. CD/ANPD 31/2025) [P](https://www.in.gov.br/web/dou/-/resolucao-cd/anpd-n-31-de-22-de-dezembro-de-2025-677950080).

**Papéis**
- O papel depende da atividade real, e um contrato controlador–operador é "uma boa prática" (Guia de Agentes de Tratamento da ANPD) [P](https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes/guia_agentes_de_tratamento_e_encarregado___defeso_eleitoral.pdf/@@display-file/file).
- O operador responde solidariamente se descumprir a lei ou as instruções (art. 42, § 1º, I).
- Como cada agregador se posiciona:
  - Belvo: "atua majoritariamente como Controladora" [P](https://belvo.com/pt-br/politica-de-privacidade/);
  - Pluggy: o app "owns the connection, and Pluggy processes the data on its behalf" [P](https://docs.pluggy.ai/en/docs/get-started/delete-account);
  - Polp: "operadora de dados em nome do Cliente" [P](https://polp.com.br/termos).

**Retenção e direitos**
- O tratamento termina, entre outros casos, com a revogação do consentimento (art. 15), e os dados são eliminados, salvo exceções como obrigação legal ou anonimização (art. 16).
- Direitos do titular: art. 18. Prazo para a declaração completa: até 15 dias (art. 19).
- Agente de pequeno porte tem prazo em dobro (Res. CD/ANPD 2/2022, arts. 14 e 15) [P](https://www.in.gov.br/en/web/dou/-/resolucao-cd/anpd-n-2-de-27-de-janeiro-de-2022-376562019).

**Segurança e incidentes** — Res. CD/ANPD 15/2024 [P](https://www.in.gov.br/en/web/dou/-/resolucao-cd/anpd-n-15-de-24-de-abril-de-2024-556243024)
- "Dados financeiros" é critério de incidente relevante (art. 5º).
- Comunicar a ANPD e o titular em 3 dias úteis, com complemento em 20 dias úteis (pequeno porte: em dobro).
- Registrar todo incidente por no mínimo 5 anos.

**Encarregado (DPO) e pequeno porte**
- A Res. CD/ANPD 18/2024 regula o encarregado [P](https://www.in.gov.br/en/web/dou/-/resolucao-cd/anpd-n-18-de-16-de-julho-de-2024-572632074).
- Microempresa, empresa de pequeno porte e startup ficam dispensadas, mas devem manter canal de contato com o titular. Também têm registro simplificado ([modelo](https://www.gov.br/anpd/pt-br/centrais-de-conteudo/modelo_de_ropa_para_atpp.pdf/@@display-file/file)) e prazos em dobro (Res. CD/ANPD 2/2022).
- A dispensa **não** vale para tratamento de "alto risco" (art. 4º), que exige:
  - um critério geral (larga escala, ou tratamento que possa afetar direitos; a norma cita "fraudes financeiras");
  - e um critério específico (tecnologia emergente, decisão só automatizada inclusive de perfil de consumo ou crédito, dados sensíveis, ou dados de crianças, adolescentes ou idosos).
  - Se um app com categorização por IA ou com usuários idosos se enquadra: (não verificado).

**RIPD (relatório de impacto)**
- Não há obrigação geral. A ANPD "poderá determinar" (art. 38) e recomenda fazer quando há alto risco [P](https://www.gov.br/anpd/pt-br/canais_atendimento/agente-de-tratamento/relatorio-de-impacto-a-protecao-de-dados-pessoais-ripd).

**Transferência internacional**
- Res. CD/ANPD 19/2024 [P](https://www.gov.br/anpd/pt-br/acesso-a-informacao/institucional/atos-normativos/regulamentacoes_anpd/resolucao-cd-anpd-no-19-de-23-de-agosto-de-2024): cláusulas-padrão adotadas sem alteração e página em português sobre a transferência.
- União Europeia e Espaço Econômico Europeu foram reconhecidos como adequados (Res. CD/ANPD 32/2026) [P](https://www.in.gov.br/web/dou/-/resolucao-n-32-de-26-de-janeiro-de-2026-683334547). Os EUA não têm decisão de adequação.
- Onde ficam os dados dos agregadores:
  - Belvo: em "data centers de terceiros localizados nos Estados Unidos" [P](https://belvo.com/pt-br/politica-de-privacidade/);
  - Pluggy: a política admite processamento fora do Brasil [P](https://www.pluggy.ai/docs/politica-de-privacidade.pdf);
  - Polp: usa a OpenAI (EUA) para categorizar [P](https://polp.com.br/subprocessadores).

**Outras normas**
- **Marco Civil, art. 15.** Guardar registros de acesso ao app por 6 meses [P](https://www.planalto.gov.br/ccivil_03/_ato2011-2014/2014/lei/l12965.htm).
- **Responsabilidade.** CDC, art. 14: responsabilidade "independentemente da existência de culpa"; a LGPD manda aplicar a lei de consumo (art. 45).
- **Multa da LGPD.** Até 2% do faturamento, limitada a R$ 50 milhões por infração (art. 52, II).
- **Fiscalização.** O plano de prioridades da ANPD para 2026–2027 inclui dados financeiros [P](https://www.in.gov.br/web/dou/-/resolucao-cd/anpd-n-30-de-23-de-dezembro-de-2025-677947163).
- **Sigilo bancário (LC 105/2001).** O dever é das "instituições financeiras" e não há violação com "consentimento expresso dos interessados" [P](https://www.planalto.gov.br/ccivil_03/leis/lcp/lcp105.htm). Se alcança o app: (não verificado).

### 5.3 Contratos e políticas dos agregadores

**Belvo** — Condições Gerais, atualizadas em 19/06/2025 [P](https://belvo.com/pt-br/terms-service/)
- **Obrigações do cliente:**
  - 4.1(c): "Manter, segundo as melhores práticas de mercado, estrutura de segurança de informação, proteção de dados e compliance";
  - 4.1(d): obter "licenças, certificados e/ou autorizações";
  - 4.1(m): link recorrente "não deverá ser removido ou manipulado de qualquer forma, pelo prazo mínimo de 90 (noventa) dias corridos a partir de sua criação";
  - 11.3: tratar os dados conforme a LGPD, incluindo "obtenção de consentimento dos Usuários Finais, quando aplicável".
- **Retenção e incidentes:**
  - 11.8.1: dados transacionais "365 dias (após a coleta do dado) ou 30 dias após a rescisão";
  - 11.10: exclusão "30 dias corridos após o fim do contrato ou 30 dias após solicitação", por "crypto shredding";
  - 11.11.2: notificação de incidente no "Padrão Belvo de 72 horas".
- **Prazo e pagamento:**
  - 6.1: vigência de 12 meses, renovação automática, aviso prévio de 60 dias;
  - 5.3: reajuste anual por IPCA;
  - 6.3: "Em caso de rescisão deste Contrato pela Belvo, o Cliente pagará a integralidade dos valores vincendos".
- **Política de privacidade** [P](https://belvo.com/pt-br/politica-de-privacidade/):
  - a Belvo é "majoritariamente" controladora;
  - os dados ficam nos EUA;
  - "No caso dos Dados obtidos através do Open Finance", a Belvo pode compartilhar com a "FAIR ISAAC DO BRASIL LTDA." (FICO) "com a finalidade exclusiva de que a FICO preste serviços relacionados ao cálculo de score de crédito".

**Pluggy**
- **Produção.** Exige formulário de "Due Diligence" de compliance com upload de documentos ("extratos e contratos") e checklist "Solicitar Acesso à Produção" [P](https://docs.pluggy.ai/pt/changelog).
- **Gestão de consentimento.** O app deve oferecer sempre a exclusão da conexão, "to be in compliance with Open Banking regulations" [P](https://docs.pluggy.ai/en/docs/integration-checklist/consent-management).
- **Proibição.** "Batch process are prohibited due to abusive usage of the API" [P](https://docs.pluggy.ai/en/docs/integration-checklist/data-sync).
- **Exclusão.** Pedidos levam "cerca de 15 dias" (política de privacidade) [P](https://www.pluggy.ai/docs/politica-de-privacidade.pdf).
- **Contrato com o cliente.** Não é público. Os termos públicos são para o usuário final [P](https://www.pluggy.ai/legal).

**Polp**
- Uso comercial só para CNPJ ativo.
- O cliente deve guardar evidência de aceite (data, hora, versão, IP etc.) no produto DDA [P](https://polp.com.br/termos).

**Klavi**
- Exige um consentimento LGPD separado para repassar dados aos parceiros [P](https://docs.klavi.ai/connect/quick-start).

### 5.4 Lojas de apps

- **App Store** (diretrizes atualizadas em 16/10/2026) [P](https://developer.apple.com/app-store/review/guidelines/):
  - 5.1.1(ix): apps em "highly regulated fields (such as banking and financial services…) or that require sensitive user information should be submitted by a legal entity that provides the services, and not by an individual developer";
  - 3.2.1(viii): "Apps used for financial trading, investing, or money management should be submitted by the financial institution performing such services";
  - se um app de orçamento se enquadra: (não verificado).
- **Google Play:**
  - "Financial products and services" exigem conta de organização com D-U-N-S [P](https://support.google.com/googleplay/android-developer/answer/10788890);
  - todo app preenche a declaração de recursos financeiros [P](https://support.google.com/googleplay/android-developer/answer/13849271);
  - contas pessoais novas precisam de teste fechado com 12 testadores por 14 dias antes de publicar [P](https://support.google.com/googleplay/android-developer/answer/14151465);
  - se um app de orçamento conta como "financial products and services": (não verificado).

---

## 6. O que o Organizze usa

**Resposta: Belvo**, para o consentimento, a coleta de dados e parte da categorização. Evidências lidas em 26/10/2026:

| # | Evidência | O que mostra | Data | Fonte |
|---|---|---|---|---|
| 1 | Artigo "A Conexão Bancária é segura?" | "A Conexão Bancária é feita em parceria com a Belvo, uma empresa regulamentada pelo Banco Central… responsável por realizar o processo de consentimento aos dados" | Editado em 04/05/2025; atualizado em 23/07/2026 | [artigo](https://ajuda.organizze.com.br/hc/pt-br/articles/34237366829715), [JSON da API](https://ajuda.organizze.com.br/api/v2/help_center/pt-br/articles/34237366829715.json) |
| 2 | Artigos "Como sincronizar minha conta do Nubank / Itaú / BB / Bradesco / Caixa / Santander" | "seja direcionado para o ambiente da Belvo, que é a ponte entre o Organizze e sua instituição bancária" | Editados em 16/12/2025 | [Nubank](https://ajuda.organizze.com.br/hc/pt-br/articles/34237881633811), [Itaú](https://ajuda.organizze.com.br/hc/pt-br/articles/34238292783635), [BB](https://ajuda.organizze.com.br/hc/pt-br/articles/34238862117395), [Bradesco](https://ajuda.organizze.com.br/hc/pt-br/articles/34238441725203), [Caixa](https://ajuda.organizze.com.br/hc/pt-br/articles/34238963726995), [Santander](https://ajuda.organizze.com.br/hc/pt-br/articles/34238712466451) |
| 3 | Artigo sobre investimentos que não aparecem | "Gerenciar consentimentos" leva ao "Meu Portal Belvo", onde o usuário refaz o consentimento para incluir investimentos | Criado em 27/08/2026; editado em 08/09/2026 | [artigo](https://ajuda.organizze.com.br/hc/pt-br/articles/54857293420307) |
| 4 | Artigo "O que é o Open Finance?" | "a Conexão Bancária do Organizze utiliza a Belvo como parceira" | 04/05/2025 | [artigo](https://ajuda.organizze.com.br/hc/pt-br/articles/34236885176467) |
| 5 | Blog "A Organizze faz empréstimo?" | "realiza conexões seguras com os bancos por meio da Belvo" | 19/03/2026 | [blog](https://www.organizze.com.br/blog/emprestimos-e-seguros/organizze-emprestimo/) |
| 6 | JavaScript público de auth.organizze.com.br (para onde redireciona app.organizze.com.br) | `window.belvoSDK.createWidget(…, integration_type:"openfinance", …)`, componentes `BelvoWidget` e `BelvoHostedWidget`, campos `belvo_link_intent[cpf]` e `belvo_link_intent[cnpj]` | Servido em 26/10/2026 | [bundle](https://auth.organizze.com.br/assets/app-63763c1a9dcc5d639f088f6cf2ae9dc6.js?vsn=d) |
| 7 | JavaScript público carregado por app.organizze.com.br/login | Objeto `bank_conn_belvo_transaction` com `mcc`, `category`, `subcategory`, `merchant.name` e `counterparty.name`; etapa de sugestão de categoria "Etapa 6b — Open Finance — categoria Belvo → grupo" | Servido em 26/10/2026 | [appModules](https://app.organizze.com.br/zze-front/eeaf9dd00354/js/zze-appModules-min.js), [application](https://app.organizze.com.br/assets/application-deb85560b83838480d09cf7f2e5c044af2317f28c25df0cd1332b0e1d9ed998f.js) |
| 8 | Buscas sem resultado para Pluggy | Zero ocorrências de "pluggy" nos bundles. Na central, a busca por "pluggy" só trouxe dois falsos positivos (a grafia "Pluggins/Plugins" em artigos sobre ChatGPT), contra 9 artigos que citam a Belvo. O Organizze não aparece entre os logos de clientes da Pluggy. | 26/10/2026 | [busca "belvo"](https://ajuda.organizze.com.br/api/v2/help_center/articles/search.json?query=belvo), [busca "pluggy"](https://ajuda.organizze.com.br/api/v2/help_center/articles/search.json?query=pluggy), [Pluggy](https://www.pluggy.ai/) |
| 9 | **Contra-evidência:** OpenAPI pública do Organizze | `AccountConnection` com `pluggy_account_id` (obrigatório) e `pluggy_item_id`, "Serialized directly from the database row, so the production payload may include additional provider-owned columns" | Repositório de 2026 | [OpenAPI](https://github.com/organizze/agent-tools/blob/main/openapi/organizze-rest-v2.yaml), [docs](https://developers.organizze.com.br/) |
| 10 | Política de privacidade do Organizze | Fala só em "parceiros homologados", sem nome | 04/2026 | [política](https://www.organizze.com.br/politicas-de-privacidade/) |

**Conclusão**
- A central de ajuda atual e o código servido hoje mostram o widget e o portal de consentimento da Belvo em produção, no modo Open Finance.
- Os nomes `pluggy_*` na API não têm explicação pública. Hipóteses: legado de uma integração anterior ou um segundo provedor (não verificado).
- **Ponto em aberto.** O Organizze lista Inter, C6, Porto Bank, Rico, PagBank e BRB entre os bancos conectáveis (artigo editado em 21/10/2026) [P](https://ajuda.organizze.com.br/hc/pt-br/articles/34237078723091). Esses bancos não estão na lista pública da Belvo [P](https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-institutions). Se a lista da Belvo está desatualizada ou se o Organizze usa outro provedor para eles: (não verificado).

**Fatos operacionais do Organizze confirmados na central atual** (complementam o documento anterior)
- Atualização automática "a cada 24 horas", +2 buscas por dia (Conectado) ou +4 (Plus). O banco "tem até 48h para enviar as informações via Open Finance" [P](https://ajuda.organizze.com.br/hc/pt-br/articles/34229891699475), [P](https://ajuda.organizze.com.br/hc/pt-br/articles/47028882431891).
- Lançamentos futuros (previstos) são permitidos em contas e cartões conectados, com conciliação sempre revisada pelo usuário: "A conciliação não acontece sozinha" (artigo de 30/09/2026). Ainda não existem transferências previstas [P](https://ajuda.organizze.com.br/hc/pt-br/articles/55219456237459).
- Dados de investimentos também são importados. A conexão PJ só existe no Conectado Plus [P](https://ajuda.organizze.com.br/hc/pt-br/articles/34229891699475).

---

## 7. O que não deu para verificar

**Preços e contratos**
- Pluggy:
  - o que conta como "conexão ativa";
  - o preço por requisição acima do volume;
  - se o CNPJ é exigido formalmente;
  - o contrato de cliente, que não é público.
- Belvo:
  - a cobrança por uso (por link, atualização ou recurso);
  - se um app de finanças pessoais se enquadra como "fintech" no plano Launch;
  - a duração do programa para startups.
- Klavi, Lina, Iniciador, Celcoin e Cumbuca: todos são "fale com vendas". Também não confirmei se o Iniciador aceita clientes não regulados.
- TecnoSpeed: o credenciamento e o parceiro regulado.
- Revendas (Polp, Banco MCP): como ficam diante da proibição de repasse a terceiros da RC 21 (art. 38, XIII).

**RC 21/2026**
- A data de publicação no DOU.
- Os requisitos técnicos e operacionais para parceiros, a documentação do auditor e as diretrizes de consentimento, que o BCB ainda vai definir.
- O custo do auditor independente registrado na CVM e quem paga.
- Se consentimentos existentes terão de ser refeitos (o art. 52-A fala em "adequação… dos consentimentos existentes").

**Cobertura e qualidade**
- Suporte real da Belvo a Inter e C6.
- Que nome aparece no app do banco na tela de consentimento.
- Se os cartões Santander seguem disponíveis após o "Santander Cartões (798)" sair do Open Finance na Pluggy.

**Open Finance**
- Quando a API de cartão v2.4.0 (com parcelas futuras) vira obrigatória e quantos bancos já a expõem.
- Se a defasagem máxima de 5 minutos vale para o cartão.
- Quando o fluxo desacoplado e a renovação sem redirecionamento após expiração (CIBA) entram em produção.
- Números oficiais atuais do painel. O dashboard é montado por JavaScript; só há o número secundário de 208,79 milhões de consentimentos ativos em 31/07/2026 [S](https://www.letsmoney.com.br/noticias/open-finance-208-milhoes-consentimentos-governanca/).

**Participante direto**
- O capital mínimo exato (classificação de investimento e captação de uma ITP).
- Se um app de finanças pessoais conta como "agregação de dados" para o adicional de R$ 5 milhões.
- O prazo máximo de análise do BCB.
- O valor em reais da contribuição à Estrutura de Governança.

**Scraping e plataformas**
- A posição formal da Febraban e bloqueios técnicos dos bancos.
- Se o Google concede a exceção de SMS a apps de orçamento, já que existe a leitura de notificações.
- Se os gatilhos "Mensagem" e "Transação" do Atalhos funcionam bem com bancos e cartões brasileiros.
- Se um app de orçamento se enquadra nas regras da App Store 5.1.1(ix) e 3.2.1(viii) e na categoria "financial products and services" do Google Play.

**Arquivos**
- OFX de conta em fonte oficial para Itaú, Banco do Brasil, Santander, Caixa e Bradesco PF.
- Formatos do XP.
- Formato de exportação da fatura de cartão na maioria dos bancos.

**LGPD**
- A base legal recomendada para "conectar banco".
- Se o app vira "alto risco" por usar IA na categorização ou ter usuários idosos.
- Se dados de transações podem revelar dados sensíveis (art. 11, § 1º).
- Se a LC 105/2001 se aplica ao app.
- Onde a Pluggy hospeda os dados.

**Organizze**
- Por que a API tem campos `pluggy_*`.
- Como Inter, C6, Porto Bank, Rico, PagBank e BRB são conectados, se não estão na lista pública da Belvo.

---

## 8. Fontes (acessadas em 26/10/2026)

### 8.1 Normas (BCB, CMN, ANPD, leis)
- Resolução Conjunta nº 21, de 22/10/2026 — https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=21 (texto lido via https://www.bcb.gov.br/api/conteudo/app/normativos/exibenormativo?p1=Resolu%C3%A7%C3%A3o%20Conjunta&p2=21)
- Resolução Conjunta nº 1/2020, texto consolidado (v9, inclui RC 4/2022, 7/2023, 10/2024 e 15/2025) — https://normativos.bcb.gov.br/Lists/Normativos/Attachments/51028/Res_Conj_0001_v9_L.pdf
- Resolução Conjunta nº 14/2025 (capital mínimo) — https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=14
- Resolução BCB nº 517/2025 (categorias e adicional de serviços) — https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=517
- Resolução Conjunta nº 17/2025 (nomenclatura) — https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20Conjunta&numero=17
- Resolução BCB nº 32/2020 (requisitos técnicos; art. 6º-A) — https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=32
- Resolução BCB nº 80/2021 — https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=80
- Resolução BCB nº 81/2021 — https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Resolu%C3%A7%C3%A3o%20BCB&numero=81
- Instrução Normativa BCB nº 485/2024 (contribuição à governança) — https://www.bcb.gov.br/estabilidadefinanceira/exibenormativo?tipo=Instru%C3%A7%C3%A3o%20Normativa%20BCB&numero=485
- Lei 13.709/2018 (LGPD) — https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm
- Lei 12.965/2014 (Marco Civil) — https://www.planalto.gov.br/ccivil_03/_ato2011-2014/2014/lei/l12965.htm
- LC 105/2001 (sigilo bancário) — https://www.planalto.gov.br/ccivil_03/leis/lcp/lcp105.htm
- Resolução CD/ANPD nº 2/2022 — https://www.in.gov.br/en/web/dou/-/resolucao-cd/anpd-n-2-de-27-de-janeiro-de-2022-376562019
- Resolução CD/ANPD nº 15/2024 — https://www.in.gov.br/en/web/dou/-/resolucao-cd/anpd-n-15-de-24-de-abril-de-2024-556243024
- Resolução CD/ANPD nº 18/2024 — https://www.in.gov.br/en/web/dou/-/resolucao-cd/anpd-n-18-de-16-de-julho-de-2024-572632074
- Resolução CD/ANPD nº 19/2024 — https://www.gov.br/anpd/pt-br/acesso-a-informacao/institucional/atos-normativos/regulamentacoes_anpd/resolucao-cd-anpd-no-19-de-23-de-agosto-de-2024
- Resolução CD/ANPD nº 30/2025 — https://www.in.gov.br/web/dou/-/resolucao-cd/anpd-n-30-de-23-de-dezembro-de-2025-677947163
- Resolução CD/ANPD nº 31/2025 — https://www.in.gov.br/web/dou/-/resolucao-cd/anpd-n-31-de-22-de-dezembro-de-2025-677950080
- Resolução CD/ANPD nº 32/2026 — https://www.in.gov.br/web/dou/-/resolucao-n-32-de-26-de-janeiro-de-2026-683334547
- Guia de Agentes de Tratamento (ANPD) — https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes/guia_agentes_de_tratamento_e_encarregado___defeso_eleitoral.pdf/@@display-file/file
- RIPD (ANPD) — https://www.gov.br/anpd/pt-br/canais_atendimento/agente-de-tratamento/relatorio-de-impacto-a-protecao-de-dados-pessoais-ripd
- Modelo de registro para pequeno porte (ANPD) — https://www.gov.br/anpd/pt-br/centrais-de-conteudo/modelo_de_ropa_para_atpp.pdf/@@display-file/file

### 8.2 Open Finance Brasil (especificações e governança)
- Limites operacionais — https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/17924220
- Referência (tabela por endpoint, v26) — https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/17957025
- Consentimento (regras de API, PJ/MEI) — https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/219480491
- Guia de experiência (prazo do consentimento) — https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/2172421135
- Renovação na receptora — https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/2172422374
- Múltiplas alçadas PJ — https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/2172423431
- Certificação — https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/280297514
- Changelog da API de cartão — https://openfinancebrasil.atlassian.net/wiki/spaces/OF/pages/1739292717
- API de cartão v2.3.1 — https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/credit-cards/2.3.1.yml
- API de cartão v2.4.0 — https://raw.githubusercontent.com/OpenBanking-Brasil/all-services-repo/refs/heads/main/api_credit-cards-accounts_-_open_finance_brasil/2.4.0.yaml
- API de contas v2.4.2 — https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/accounts/2.4.2.yml
- API de consentimentos v3.3.1 — https://github.com/OpenBanking-Brasil/openapi/blob/main/swagger-apis/consents/3.3.1.yml
- Diretório de participantes (API pública) — https://data.directory.openbankingbrasil.org.br/participants
- Quem participa — https://openfinancebrasil.org.br/quem-participa/
- Taxas de certificação da OpenID Foundation — https://openid.net/certification/fees/

### 8.3 Agregadores e revendas
- **Pluggy:**
  - preços: https://www.pluggy.ai/precos
  - MeuPluggy: https://www.pluggy.ai/meu-pluggy e https://meu.pluggy.ai/api-guide
  - FAQ: https://docs.pluggy.ai/en/docs/get-started/faq
  - changelog: https://docs.pluggy.ai/pt/changelog
  - limites: https://docs.pluggy.ai/en/docs/open-finance/rate-limits
  - parcelas: https://docs.pluggy.ai/en/docs/products/credit-card-installments
  - faturas: https://docs.pluggy.ai/en/docs/products/credit-card-bills
  - ciclo de fatura: https://docs.pluggy.ai/en/docs/products/credit-card-statement-cycle
  - campos OF x direto: https://docs.pluggy.ai/en/docs/connections/open-finance-vs-direct-fields
  - consentimentos: https://docs.pluggy.ai/en/docs/connections/consents
  - item e auto-sync: https://docs.pluggy.ai/en/docs/connections/item
  - ciclo de vida: https://docs.pluggy.ai/en/docs/connections/item-lifecycle
  - sincronização: https://docs.pluggy.ai/en/docs/integration-checklist/data-sync
  - gestão de consentimento: https://docs.pluggy.ai/en/docs/integration-checklist/consent-management
  - categorização: https://docs.pluggy.ai/en/docs/products/transaction-categorization
  - webhooks: https://docs.pluggy.ai/en/docs/developer-tools/webhooks-ref
  - exclusão: https://docs.pluggy.ai/en/docs/get-started/delete-account
  - cobertura: https://docs.pluggy.ai/en/docs/open-finance/institutions-coverage
  - produtos Open Finance: https://docs.pluggy.ai/docs/open-finance-regulated
  - status (API): https://status.pluggy.ai/api/status
  - termos: https://www.pluggy.ai/legal
  - privacidade: https://www.pluggy.ai/docs/politica-de-privacidade.pdf
  - snapshots de preço: https://web.archive.org/web/20230605170413/https://pluggy.ai/pricing , https://web.archive.org/web/20240528195836/https://www.pluggy.ai/pricing , https://web.archive.org/web/20250119093813/https://www.pluggy.ai/pricing
- **Belvo:**
  - planos BR: https://belvo.com/pt-br/planos-precos/
  - planos globais: https://belvo.com/plans-and-pricing/
  - startups: https://belvo.com/pt-br/startups/
  - termos: https://belvo.com/pt-br/terms-service/
  - privacidade: https://belvo.com/pt-br/politica-de-privacidade/
  - instituições: https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-institutions
  - limites: https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-data-retrieval-limits
  - introdução: https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-introduction
  - Meu Portal Belvo: https://developers.belvo.com/products/aggregation_brazil/aggregation-brazil-mybelvoportal
  - widget: https://developers.belvo.com/products/aggregation_brazil/ofda-widget-introduction e https://developers.belvo.com/products/aggregation_brazil/ofda-widget-branding-and-customization
  - links: https://developers.belvo.com/developer_resources/resources-links-overview
  - webhooks: https://developers.belvo.com/developer_resources/resources-webhooks-aggregation
  - enriquecimento: https://developers.belvo.com/developer_resources/enrichment-overview
  - help center: https://support.belvo.com/hc/en-us/articles/22296314310941
  - blog "licenciamento proxy": https://belvo.com/pt-br/blog/licenciamento-proxy-open-finance-como-funciona/
  - blog "licença": https://belvo.com/pt-br/blog/licenca-open-finance-quanto-custa-quando-vale-fintechs/
  - home: https://belvo.com/pt-br/
  - snapshots: http://web.archive.org/web/20220630220421/https://belvo.com/pt-br/planos-precos/ , http://web.archive.org/web/20251102121036/https://belvo.com/pt-br/planos-precos/
- **Klavi:** https://www.klavi.ai/ ; https://docs.klavi.ai/connect/participant-institutions ; https://docs.klavi.ai/connect/quick-start ; https://docs.klavi.ai/connect/concepts ; https://docs.klavi.ai/connect/consent-permissions ; https://docs.klavi.ai/connect/lqUr-overview ; https://docs.klavi.ai/connect/release-history ; https://docs.klavi.ai/klaas/transactions-categorization
- **Lina Open X:** https://linaopenx.com.br/data-link ; https://consentimento.linaopenx.com.br/docs/termos-e-condicoes.pdf
- **Iniciador:** https://iniciador.com.br/produtos/dados-bancarios ; https://iniciador.com.br/legal ; https://iniciador.com.br/en/infraestrutura/receptora-de-dados
- **Celcoin:** https://www.celcoin.com.br/open-finance/financial-data/ ; https://developers.celcoin.com.br/docs/receptor-de-dados
- **Cumbuca:** https://www.cumbuca.com/
- **Polp:** https://polp.com.br/ ; https://polp.com.br/termos ; https://polp.com.br/subprocessadores ; https://polp.com.br/docs/celcoin/termos ; https://api.polp.com.br/api/v1/billing/plans ; https://api.polp.com.br/api/v1/billing/tiers
- **Banco MCP:** https://banco.mcp.ai/ ; https://banco.mcp.ai/termos
- **Estrangeiros:** https://plaid.com/docs/api/link/ ; https://docs.fintoc.com/docs/welcome ; https://syncfy.com/en/coverage

### 8.4 Plataformas (Apple e Google)
- **Apple:**
  - App Store Review Guidelines: https://developer.apple.com/app-store/review/guidelines/
  - segurança/sandbox: https://support.apple.com/guide/security/security-of-runtime-process-sec15bfe098e/web
  - notificações entregues: https://developer.apple.com/documentation/usernotifications/unusernotificationcenter/getdeliverednotifications(completionhandler:)
  - filtro de SMS: https://developer.apple.com/documentation/identitylookup/sms-and-mms-message-filtering
  - Atalhos: https://support.apple.com/guide/shortcuts/apdd711f9dff/ios , https://support.apple.com/guide/shortcuts/transaction-trigger-apd65c67538a/ios , https://support.apple.com/guide/shortcuts/create-a-new-personal-automation-apdfbdbd7123/ios
  - App Intents: https://developer.apple.com/documentation/appintents
  - FinanceKit: https://developer.apple.com/financekit/
- **Google:**
  - SMS/Call Log: https://support.google.com/googleplay/android-developer/answer/10208820
  - permissões sensíveis: https://support.google.com/googleplay/android-developer/answer/9888170
  - Dados do Usuário: https://support.google.com/googleplay/android-developer/answer/10144311
  - conta de organização: https://support.google.com/googleplay/android-developer/answer/10788890
  - declaração financeira: https://support.google.com/googleplay/android-developer/answer/13849271
  - teste fechado: https://support.google.com/googleplay/android-developer/answer/14151465
  - NotificationListenerService: https://developer.android.com/reference/android/service/notification/NotificationListenerService
  - Settings: https://developer.android.com/reference/android/provider/Settings
  - Android 15: https://developer.android.com/about/versions/15/behavior-changes-all
  - verificação de desenvolvedor: https://developer.android.com/developer-verification

### 8.5 Bancos (exportação) e concorrentes
- **Bancos:**
  - Nubank: https://nubank.com.br/perguntas ; https://nubank.com.br/contratos/termos-e-condicoes-conta-nubank
  - Itaú: https://www.itau.com.br/atendimento-itau/para-voce/conta-corrente/como-consultar-o-meu-extrato-itau-e-o-que-significam-as-siglas-do-extrato
  - Bradesco: https://www.institucional.bradesco.com.br/simulador_bradesco_net_empresa/conteudo/perfil_corporate/extrato/conta-poupanca_extrato-mensal-periodo2b.asp
  - Inter: https://ajuda.inter.co/conta-digital-pessoa-fisica-e-mei/como-acessar-o-extrato-da-conta-digital-pf-kids-ou-you ; https://ajuda.inter.co/cartao/como-acessar-baixar-ou-emitir-a-segunda-via-da-fatura
  - C6: https://www.c6bank.com.br/blog/como-receber-o-extrato-da-sua-conta-do-c6-bank-por-e-mail ; https://www.c6bank.com.br/blog/como-receber-a-fatura-do-meu-cartao-do-c6-bank-em-pdf
  - BTG: https://banking.btgpactual.com/ajuda/contacorrente/extrato ; https://banking.btgpactual.com/ajuda/cartao/fatura
  - Mercado Pago: https://www.mercadopago.com.br/blog/como-obter-extrato-mercado-pago
  - PicPay: https://meajuda.picpay.com/hc/pt-br/articles/4408499088525 ; https://meajuda.picpay.com/hc/pt-br/articles/22196611297171
- **Mobills:** https://mobills.zendesk.com/hc/pt-br/articles/360051695074 ; https://mobills.zendesk.com/hc/pt-br/articles/360052472853 ; https://mobills.zendesk.com/hc/pt-br/articles/360052620713

### 8.6 Organizze
- Central de ajuda (lida pela API pública do Zendesk; o HTML é bloqueado):
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34237366829715
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34229891699475
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34237078723091
  - https://ajuda.organizze.com.br/hc/pt-br/articles/47028882431891
  - https://ajuda.organizze.com.br/hc/pt-br/articles/49964437896979
  - https://ajuda.organizze.com.br/hc/pt-br/articles/55219456237459
  - https://ajuda.organizze.com.br/hc/pt-br/articles/54857293420307
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34236885176467
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34237881633811
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34238292783635
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34238862117395
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34238441725203
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34238963726995
  - https://ajuda.organizze.com.br/hc/pt-br/articles/34238712466451
  - https://ajuda.organizze.com.br/hc/pt-br/articles/6373504617363
  - https://ajuda.organizze.com.br/hc/pt-br/articles/360049302233
  - https://ajuda.organizze.com.br/hc/pt-br/articles/5994104693395
- Buscas na API da central:
  - https://ajuda.organizze.com.br/api/v2/help_center/articles/search.json?query=belvo
  - https://ajuda.organizze.com.br/api/v2/help_center/articles/search.json?query=pluggy
- Blog: https://www.organizze.com.br/blog/emprestimos-e-seguros/organizze-emprestimo/
- Política de privacidade: https://www.organizze.com.br/politicas-de-privacidade/
- JavaScript público:
  - https://auth.organizze.com.br/assets/app-63763c1a9dcc5d639f088f6cf2ae9dc6.js?vsn=d
  - https://app.organizze.com.br/zze-front/eeaf9dd00354/js/zze-appModules-min.js
  - https://app.organizze.com.br/assets/application-deb85560b83838480d09cf7f2e5c044af2317f28c25df0cd1332b0e1d9ed998f.js
- API:
  - https://developers.organizze.com.br/
  - https://github.com/organizze/agent-tools/blob/main/openapi/organizze-rest-v2.yaml

### 8.7 Fontes secundárias (só pistas)
- TabNews (21/05/2026) — https://www.tabnews.com.br/GuilhermeVieira/estou-desenvolvendo-um-app-de-financas-pessoais-e-nao-consigo-pagar-o-open-finance-pluggy-r2-5k-mes-belvo-r6k-mes-tecnospeed-r1-5k-de-entrada-r540
- TabNews (17/06/2026) — https://www.tabnews.com.br/GuilhermeVieira/resolvi-o-problema-do-open-finance-caro-que-postei-aqui-achei-uma-alternativa-e-ja-esta-em-producao
- Let's Money (BC e venda de dados) — https://www.letsmoney.com.br/noticias/bc-proibir-venda-dados-open-finance/
- Let's Money (208 milhões de consentimentos) — https://www.letsmoney.com.br/noticias/open-finance-208-milhoes-consentimentos-governanca/
- Finsiders (parcerias > 20% dos consentimentos) — https://finsidersbrasil.com.br/?p=371500
- Chrome Unboxed (configurações restritas do Android 13) — https://chromeunboxed.com/android-13-restricted-setting-feature-blocks-sideloaded-apps-from-notifications/
- Meu Contador Online (guias de OFX de Itaú, Bradesco, BB, Caixa e Santander) — URLs na tabela da seção 2.5
- Tecnoblog (fatura do Nubank) — https://tecnoblog.net/responde/como-obter-uma-versao-impressa-da-fatura-do-cartao-nubank/
