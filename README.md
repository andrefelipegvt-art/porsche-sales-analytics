# Porsche Sales Analytics

Dashboard comercial interativa criada para o desafio da DIO **Criando Agentes de Tratamento de Dados**. O projeto transforma uma planilha sanitizada com 100 registros de vendas Porsche em uma página HTML interativa, com indicadores, gráficos e filtros.

## Dashboard publicada

**Status:** ainda não publicada no GitHub Pages.

Após publicar o repositório, substitua o endereço abaixo pelo link real da dashboard:

`https://SEU-USUARIO.github.io/porsche-sales-analytics/`

> O endereço acima é um modelo, não um link publicado. O link do repositório público e o link do GitHub Pages são diferentes; informe ambos ao submeter o desafio.

## Perguntas de negócio

### 1. Quais modelos Porsche apresentam o maior preço total registrado?

Compara a soma dos preços registrados e o número de registros por modelo. Na base analisada, o **911 Dakar** aparece em primeiro lugar, com **US$ 810.600,00 em 3 registros**. A soma dos preços não deve ser interpretada automaticamente como receita realizada.

### 2. Quais estados concentram mais registros e maior preço total registrado?

A dashboard apresenta dois rankings para evitar misturar métricas distintas:

- **Quantidade de registros:** Califórnia (CA), com 16 registros.
- **Preço total registrado:** Texas (TX), com US$ 2.023.400,50 em 14 registros.

Esse contraste mostra por que volume e valor devem ser analisados separadamente.

### 3. Como o preço total registrado e a quantidade de vendas se distribuem por método de pagamento?

Compara volume de registros e soma de preços por modalidade. **Wire Transfer** lidera os dois recortes, com 26 registros e US$ 3.979.450,00 em preços registrados. As categorias sanitizadas foram mantidas separadas; por exemplo, `Wire Transfer`, `Bank Transfer` e `ACH Payment` não foram agrupadas sem uma regra de negócio comprovada.

## Indicadores e filtros

A página apresenta quatro indicadores no topo:

- Quantidade de registros no recorte atual;
- Preço total registrado, em USD;
- Preço médio por registro, em USD;
- Quantidade de modelos distintos no recorte.

Filtros disponíveis: modelo Porsche, estado, cidade, ano do modelo, método de pagamento e status da entrega. Os filtros podem ser combinados e os indicadores/gráficos são recalculados em conjunto.

A área de introdução inclui uma imagem de destaque de um Porsche 911 ao pôr do sol. A imagem fica embutida no HTML, preservando a entrega em arquivo único.

## Tratamento da base

A fonte é a aba `Sanitized` do arquivo disponibilizado no desafio DIO. A aba possui 100 registros e 21 colunas, com campos originais e suas versões sanitizadas.

O HTML incorpora somente os campos sanitizados usados na análise:

- `PorscheModelSanitized` → modelo;
- `ModelYearSanitized` → ano do modelo, convertido para número;
- `SalesPriceSanitized` → preço, convertido para número;
- `PayMethodSanitized` → método de pagamento;
- `CitySanitized` → cidade;
- `StateSanitized` → estado;
- `DeliveryStatusSanitized` → status da entrega;
- `SaleDateSanitized` → data sanitizada.

O identificador da venda, os nomes de clientes e os nomes de vendedores não foram incorporados ao HTML, pois não são necessários para responder às perguntas de negócio.

### Regras de análise

1. O indicador monetário é chamado **preço total registrado**, não receita realizada. A soma inclui todos os status de entrega, inclusive pendentes e cancelados.
2. Os 100 registros são preservados nas análises não temporais.
3. As 24 datas `INVALID` foram mantidas como estão. Como a primeira versão não possui gráfico temporal, nenhum registro foi excluído por causa da data. Em uma análise temporal futura, registros sem data válida devem ser excluídos somente do cálculo temporal.
4. Os preços são apresentados em dólares americanos (USD); não houve conversão cambial.
5. Categorias de pagamento e status foram preservadas conforme a versão sanitizada. Não houve consolidação de categorias apenas por semelhança de nome.
6. Valores de quilometragem suspeitos não foram corrigidos automaticamente e não são usados nas três análises principais.

## Ferramentas utilizadas

- **ChatGPT:** apoio à definição das perguntas, planejamento, geração e revisão iterativa do código.
- **HTML:** estrutura da página;
- **CSS:** identidade visual e responsividade;
- **JavaScript:** filtros, agregações, indicadores e atualização dos gráficos;
- **Chart.js 4.4.1:** gráficos interativos, carregado via CDN do jsDelivr.
- **Imagem de destaque:** visual automotivo gerado com IA, recortado para a área de cabeçalho e incorporado ao `index.html` em Base64; não exige arquivo de imagem separado.
- **GitHub Pages:** publicação estática planejada.

A primeira versão foi construída e revisada em conversa com o ChatGPT. Não foi utilizado agente com skill própria. A página depende de conexão com a internet para carregar Chart.js e a fonte web; os dados de análise ficam embutidos no arquivo HTML.

## Prompt de partida

Este é o prompt-base utilizado na conversa para organizar a análise e a construção, antes dos ajustes específicos definidos durante os testes:

> Estou desenvolvendo o projeto Porsche Sales Analytics, uma dashboard comercial interativa baseada em uma planilha com 100 vendas.
>
> Antes de gerar código, analise a estrutura dos dados sanitizados e me ajude a validar:
>
> 1. Os tipos de dados e os valores inválidos.
> 2. A definição correta de receita e ticket médio.
> 3. Três perguntas de negócio relevantes.
> 4. Os filtros necessários e como devem interagir.
>
> A dashboard será construída em um único arquivo HTML, com CSS e JavaScript, usando Chart.js para os gráficos. Quero uma interface profissional inspirada na identidade visual da Porsche, com indicadores comerciais, gráficos interativos e filtros por modelo, estado, ano do modelo, método de pagamento e status.
>
> Não invente resultados. Não descarte registros sem justificar. Explique as decisões antes de gerar o código e depois me ajude a validar cada funcionalidade. A solução deverá ser compatível com o GitHub Pages e ter instruções para publicação e documentação no README.md.

O filtro de cidade foi acrescentado na implementação para permitir um recorte geográfico mais detalhado, alinhado aos requisitos do desafio.

## Evolução do prompt e decisões incorporadas

Durante a revisão, as instruções foram refinadas para refletir as decisões de negócio e os testes:

1. Definiu-se que a soma deve ser identificada como **preço total registrado**, sem afirmar que corresponde à receita realizada.
2. Os registros são preservados, inclusive os que possuem data `INVALID`; essas datas só seriam excluídas de análises temporais.
3. Os três recortes de análise foram fixados: modelo, estado e método de pagamento, sempre comparando valor e volume quando relevante.
4. Os filtros foram definidos para atuar em conjunto, atualizando cartões e gráficos.
5. O ano do modelo foi convertido para número e a comparação dos filtros foi ajustada para preservar a seleção por ano.
6. O HTML foi limitado aos campos sanitizados necessários, sem nomes de clientes ou vendedores.
7. Foram incluídos rankings de modelos, dois rankings por estado e dois rankings por método de pagamento.

## Validação

### Comparação com a fonte

- 100 registros carregados;
- 40 modelos distintos e 33 estados distintos;
- preço total registrado: **US$ 12.827.800,50**;
- 24 datas `INVALID` preservadas;
- comparação linha a linha dos oito campos usados no HTML contra as colunas sanitizadas: **nenhuma divergência encontrada**.

### Testes funcionais

Os testes manuais iniciais reportados durante o desenvolvimento passaram:

| Teste | Resultado esperado |
|---|---:|
| Carregamento inicial | 100 registros; US$ 12.827.800,50 |
| Modelo `911 Dakar` | 3 registros; US$ 810.600,00 |
| Estado `TX` | 14 registros; US$ 2.023.400,50 |
| Pagamento `Wire Transfer` | 26 registros; US$ 3.979.450,00 |
| Status `Delivered` | 41 registros; US$ 5.078.800,50 |

Após a conversão do ano para número, também foram executadas verificações de lógica para o filtro de ano, filtros combinados e botão de limpar filtros. A sintaxe JavaScript foi validada com Node.js. A inspeção visual no navegador deve ser repetida antes da submissão final, incluindo o comportamento do filtro de ano na versão atualizada.

## Como publicar no GitHub Pages

1. Crie na sua conta do GitHub um repositório **público** chamado `porsche-sales-analytics`.
2. Envie `index.html` e `README.md` para a raiz do repositório.
3. No repositório, abra **Settings → Pages**.
4. Em **Build and deployment**, escolha **Deploy from a branch**, selecione a branch `main` e a pasta `/(root)`, depois salve.
5. Aguarde o GitHub Pages disponibilizar o site. Abra o link fornecido nas configurações e confira os gráficos, os filtros combinados e o botão de limpar filtros.
6. Substitua o endereço de exemplo no início deste README pelo endereço real e inclua uma captura de tela com pelo menos um filtro aplicado.
7. Na submissão da DIO, informe o **link do repositório público**, não apenas o endereço da dashboard.

## Estrutura do projeto

```text
porsche-sales-analytics/
├── index.html
└── README.md
```

## Observações

- Projeto educacional baseado em uma base sanitizada fornecida pela DIO.
- Os resultados representam os registros dessa base e não descrevem necessariamente vendas reais da Porsche.
- Todos os valores monetários são apresentados em USD.
