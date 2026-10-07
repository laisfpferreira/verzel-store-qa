# Verzel Store – Teste técnico de QA

Testes da entrega **VZS-142 – Cupom de desconto e frete grátis** (versão 2.3.0) da Verzel Store, feitos na API e na interface.

**Autora:** Laís Fernanda de Paula Ferreira

## Resumo dos resultados

| | Quantidade |
|---|---|
| Critérios de aceite cobertos | 11 de 11 (CA01 a CA11) |
| Requisições de API automatizadas (Postman) | 48 |
| Casos de teste na interface | 23 |
| Bugs encontrados | 3 (2 de severidade alta, 1 baixa) |
| Sugestões de melhoria | 1 |

## Bugs encontrados

| # | Bug | Critério | Severidade | Onde |
|---|---|---|---|---|
| [#1](https://github.com/laisfpferreira/verzel-store-qa/issues/1) | Frete cobrado quando o subtotal é exatamente R$ 200,00 | CA06 | Alta | API e interface |
| [#2](https://github.com/laisfpferreira/verzel-store-qa/issues/2) | API aceita mais de 5 unidades do mesmo produto | CA10 | Alta | API |
| [#3](https://github.com/laisfpferreira/verzel-store-qa/issues/3) | Item sem produtoId retorna código de erro errado e mensagem com "undefined" | Códigos de erro | Baixa | API |

**Sugestão de melhoria:** [#4](https://github.com/laisfpferreira/verzel-store-qa/issues/4) Mensagem de CEP imprecisa quando há letras.

Cada Issue traz passos para reproduzir, resultado esperado, resultado obtido e evidências.

## Estratégia de testes

1. **Análise da documentação:** cada critério de aceite virou um ou mais cenários, com foco nos **valores-limite** (ex.: subtotal de R$ 199,80, R$ 200,00 e acima de R$ 200,00; quantidade 5 e 6; CEP com 7, 8 e 9 dígitos).
2. **Testes de API (Postman):** coleção com 48 requisições e verificações automáticas. Além dos testes de cada cenário, toda resposta é validada contra a fórmula `total = subtotal - desconto + frete`, a regra do frete, o arredondamento em 2 casas e o formato padrão de erro.
3. **Testes na interface:** execução manual dos mesmos critérios na loja, incluindo a finalização da compra.
4. **Investigação das falhas:** antes de registrar um bug, cada falha foi reproduzida e isolada. Exemplo: a falha inicial do CA08 usava subtotal de exatamente R$ 200,00 e era causada pelo Bug #1. Um teste isolado (subtotal R$ 219,80 com cupom) mostrou que o CA08 funciona corretamente.

## Casos de teste na interface

| ID | Critério | Cenário | Resultado | Evidência |
|---|---|---|---|---|
| CT01 | CA01, CA02 | Cupom `bemvindo10` em minúsculo aplica 10% | ✅ Passou | [print](evidencias/testes-aprovados/ct-ca02-cupom-minusculo.png) |
| CT02 | CA02 | Cupom com espaços antes e depois é aceito | ✅ Passou | [antes](evidencias/testes-aprovados/ct-ca02-cupom-espacos-antes.png) / [depois](evidencias/testes-aprovados/ct-ca02-cupom-espacos.png) |
| CT03 | CA03 | Cupom inexistente exibe "Cupom inválido." | ✅ Passou | [print](evidencias/testes-aprovados/ct-ca03-cupom-invalido.png) |
| CT04 | CA04 | Cupom VERAO2026 exibe "Cupom expirado." | ✅ Passou | [print](evidencias/testes-aprovados/ct-ca04-cupom-expirado.png) |
| CT05 | CA05 | Após aplicar um cupom, só é possível removê-lo (não há como aplicar outro) | ✅ Passou | [print](evidencias/testes-aprovados/ct-ca02-cupom-minusculo.png) |
| CT06 | CA06 | Subtotal exatamente R$ 200,00 tem frete grátis | ❌ Falhou | [Bug #1](https://github.com/laisfpferreira/verzel-store-qa/issues/1) |
| CT07 | CA06 | Subtotal acima de R$ 200,00 (R$ 299,50) tem frete grátis | ✅ Passou | [print](evidencias/bug02-quantidade-6-api/ct-ca10-limite-carrinho.png) |
| CT08 | CA07 | Subtotal R$ 199,80 cobra frete e informa "Faltam R$ 0,20" | ✅ Passou | [print](evidencias/testes-aprovados/ct-ca07-faltante-199-80.png) |
| CT09 | CA08 | Subtotal R$ 219,80 com cupom (total R$ 197,82) mantém frete grátis | ✅ Passou | [print](evidencias/testes-aprovados/ct-recalculo-com-cupom-1.png) |
| CT10 | CA09 | Desconto não incide sobre o frete (total R$ 73,81) | ✅ Passou | [print](evidencias/testes-aprovados/ct-ca02-cupom-minusculo.png) |
| CT11 | CA01, CA07 | Remover item com cupom aplicado recalcula desconto, frete e faltante | ✅ Passou | [print](evidencias/testes-aprovados/ct-recalculo-com-cupom-2.png) |
| CT12 | CA10 | Interface bloqueia a 6ª unidade (botão "+", campo e página de produtos) | ✅ Passou | [carrinho](evidencias/bug02-quantidade-6-api/ct-ca10-limite-carrinho.png) / [produtos](evidencias/bug02-quantidade-6-api/ct-ca10-limite-produtos.png) |
| CT13 | CA10 | API recusa quantidade 6 | ❌ Falhou | [Bug #2](https://github.com/laisfpferreira/verzel-store-qa/issues/2) |
| CT14 | CA11 | Valores exibidos e retornados com 2 casas decimais | ✅ Passou | Coleção Postman |
| CT15 | Cliente | Formulário vazio exibe erro nos 3 campos | ✅ Passou | [print](evidencias/checkout/ct-checkout-f1-vazio.png) |
| CT16 | Cliente | Nome sem sobrenome é recusado | ✅ Passou | [print](evidencias/checkout/ct-checkout-f2-sem-sobrenome.png) |
| CT17 | Cliente | E-mail sem @ é recusado | ✅ Passou | [print](evidencias/checkout/ct-checkout-f3-email-invalido.png) |
| CT18 | Cliente | CEP com 7 dígitos é recusado | ✅ Passou | [print](evidencias/checkout/ct-checkout-f4-cep-7-digitos.png) |
| CT19 | Cliente | CEP com 9 dígitos é recusado | ✅ Passou | [print](evidencias/checkout/ct-checkout-f5-cep-9-digitos.png) |
| CT20 | Cliente | CEP com letra é recusado | ✅ Passou (com sugestão [#4](https://github.com/laisfpferreira/verzel-store-qa/issues/4)) | [print](evidencias/checkout/ct-checkout-f5-cep-com-letra.png) |
| CT21 | Pedido | Pedido válido é confirmado com número VZ-000000 e valores iguais ao resumo | ✅ Passou | [resumo](evidencias/checkout/ct-checkout-resumo.png) / [confirmação](evidencias/checkout/ct-checkout-f6-pedido-confirmado.png) |
| CT22 | CA06 | Pedido com subtotal R$ 200,00 é confirmado com frete grátis | ❌ Falhou | [Bug #1](https://github.com/laisfpferreira/verzel-store-qa/issues/1) |
| CT23 | Códigos de erro | Item sem produtoId retorna ITEM_INVALIDO | ❌ Falhou | [Bug #3](https://github.com/laisfpferreira/verzel-store-qa/issues/3) |

## Testes de API (Postman)

A coleção está na pasta [`postman/`](postman/), organizada em 7 pastas:

| Pasta | O que cobre |
|---|---|
| 1. Produtos | Listagem e consulta de produtos |
| 2. Cupom | CA01 a CA04: aplicação, maiúsculas/minúsculas, espaços, inválido e expirado |
| 3. Frete | CA06 a CA09: limite de R$ 200, faltante, frete antes do desconto, desconto fora do frete |
| 4. Arredondamento | CA11: somas com decimais e exemplo da documentação |
| 5. Quantidade e itens | CA10 e validações: quantidade 0, negativa, decimal, texto, item duplicado, lista vazia |
| 6. Pedidos | Pedido válido, cupom inválido/expirado, dados do cliente inválidos |
| 7. Erros gerais | JSON inválido, método não permitido, rota inexistente |

**Como executar:**

1. Instale o [Postman](https://www.postman.com/downloads/).
2. Clique em **Import** e selecione o arquivo `.json` da pasta `postman/`.
3. Clique com o botão direito na coleção e escolha **Run**.

Os resultados esperados vêm da documentação. Um teste que falha indica um possível bug, que deve ser reproduzido antes do registro.

## Comportamentos não reportados

Seguindo a seção "Sobre este ambiente" da documentação, não foram reportados como bugs: carrinho guardado só na aba do navegador, pedidos não armazenados, ausência de e-mail e cobrança, e itens fora do escopo (login, cadastro, pagamento online e consulta de pedidos).

## Estrutura do repositório

```
evidencias/
├── bug01-frete-200/                   prints do Bug #1
├── bug02-quantidade-6-api/            prints do Bug #2
├── bug03-codigo-erro-item-invalido/   prints do Bug #3
├── checkout/                          testes da finalização da compra
└── testes-aprovados/                  testes de cupom, frete e recálculo que passaram
postman/                               coleção de testes de API
```

## Uso de IA

Usei o Claude (assistente de IA da Anthropic) como apoio durante todo o teste:

- **Análise da documentação:** para levantar cenários de teste a partir dos critérios de aceite, com foco em valores-limite.
- **Coleção do Postman:** eu não tinha experiência com automação, e a IA montou a coleção com as verificações automáticas. Usei o processo para aprender a ferramenta: rodar requisições, ler respostas em JSON, interpretar os resultados dos testes e editar scripts.
- **Organização e escrita:** estrutura do repositório e rascunho dos relatórios de bug e deste README.

**O que fiz pessoalmente:** executei todos os testes na API e na interface, conferi cada resultado com a documentação, capturei as evidências e decidi o que era bug, o que era melhoria e o que era comportamento esperado.

A validação humana foi necessária: um dos testes gerados pela IA (carrinho com todos os produtos) falhou porque o valor esperado no próprio teste estava errado. Investiguei a resposta da API, confirmei que a loja estava correta, não registrei bug e corrigi o teste. Também isolei a falha do CA08 para confirmar que ela era causada pelo Bug #1, e não por um segundo bug.
