# Porsche Sales Analytics

Dashboard comercial interativa do desafio da DIO **Criando Agentes de Tratamento de Dados**. A aplicação transforma a aba sanitizada de uma planilha com 100 registros de vendas Porsche em uma página HTML com indicadores, gráficos e filtros combináveis.

## Links do projeto

- **Dashboard publicada (GitHub Pages):** https://andrefelipegvt-art.github.io/porsche-sales-analytics/
- **Repositório público:** https://github.com/andrefelipegvt-art/porsche-sales-analytics

## Perguntas de negócio

### 1. Quais modelos Porsche apresentam o maior preço total registrado?

Compara a soma dos preços registrados e a quantidade de registros por modelo. Nesta base, o **911 Dakar** aparece em primeiro lugar, com **US$ 810.600,00 em 3 registros**. O preço total registrado não deve ser interpretado automaticamente como receita realizada.

### 2. Quais estados concentram mais registros e maior preço total registrado?

A dashboard apresenta dois rankings para separar volume e valor:

- **Quantidade de registros:** Califórnia (CA), com 16 registros.
- **Preço total registrado:** Texas (TX), com US$ 2.023.400,50 em 14 registros.

O contraste mostra por que volume e valor precisam ser analisados separadamente.

### 3. Como o preço total registrado e a quantidade de vendas se distribuem por método de pagamento?

Compara o volume de registros e a soma dos preços por modalidade. **Wire Transfer** lidera os dois recortes, com 26 registros e US$ 3.979.450,00 em preços registrados. As categorias sanitizadas foram mantidas separadas; por exemplo, `Wire Transfer`, `Bank Transfer` e `ACH Payment` não foram agrupadas sem uma regra de negócio comprovada.

## Indicadores e filtros

A página apresenta quatro indicadores no topo:

- Quantidade de registros no recorte atual;
- Preço total registrado, em USD;
- Preço médio por registro, em USD;
- Quantidade de modelos distintos no recorte.

Filtros disponíveis: modelo Porsche, estado, cidade, ano do modelo, método de pagamento e status da entrega. Eles podem ser combinados e atualizam os indicadores e os gráficos em conjunto. A página também contém um botão para limpar os filtros.

O cabeçalho possui uma imagem automotiva de destaque gerada com IA e incorporada ao HTML, preservando a entrega em arquivo único.

## Tratamento da base

A fonte é a aba `Sanitized` do arquivo disponibilizado no desafio da DIO. A planilha possui 100 registros e 21 colunas, incluindo campos originais e suas versões sanitizadas.

O HTML incorpora os campos sanitizados usados na análise:

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

1. O indicador monetário é chamado **preço total registrado**, não receita realizada. A soma considera todos os status de entrega, inclusive pendentes e cancelados.
2. Os 100 registros são preservados nas análises não temporais.
3. As 24 datas `INVALID` foram mantidas. Como a versão atual não apresenta gráfico temporal, nenhum registro foi excluído por causa da data. Em uma futura análise temporal, somente os registros sem data válida devem ser excluídos dos cálculos que dependam de data.
4. Os preços são apresentados em dólares americanos (USD), sem conversão cambial.
5. As categorias de pagamento e de status foram preservadas conforme os campos sanitizados; não houve consolidação baseada apenas na semelhança dos nomes.
6. Valores de quilometragem suspeitos não foram corrigidos automaticamente e não são usados nas três análises principais.

## Validação final e evidência visual

A dashboard foi publicada no GitHub Pages e os filtros e gráficos foram testados manualmente. Para a evidência da entrega DIO, recomenda-se anexar uma captura que mostre o filtro `911 Dakar` selecionado e os indicadores do recorte (3 registros e US$ 810.600,00), além de uma captura dos gráficos. Os gráficos de quantidade usam escala inteira para evitar rótulos fracionários arredondados repetidos.

## Ferramentas e abordagem

- **ChatGPT:** apoio à definição das perguntas de negócio, diagnóstico dos dados, elaboração do prompt, geração do HTML, revisão iterativa e validação.
- **HTML:** estrutura da página;
- **CSS:** identidade visual escura, acentos vermelhos e responsividade;
- **JavaScript:** filtros, agregações, indicadores e atualização dos gráficos;
- **Chart.js 4.4.1:** gráficos interativos, carregados via CDN do jsDelivr;
- **Git e GitHub:** versionamento e publicação do código;
- **GitHub Pages:** hospedagem estática da dashboard.

O projeto foi construído por meio de interação direta com o ChatGPT, com revisão por etapas. Não foi utilizado um agente personalizado com skill própria nem o Canvas. A aplicação depende de conexão com a internet para carregar Chart.js e a fonte web; os dados de análise ficam embutidos no HTML.

## Prompt inicial

> Estou desenvolvendo o projeto Porsche Sales Analytics, uma dashboard comercial interativa baseada em uma planilha com 100 vendas.
>
> Antes de gerar código, analise a estrutura dos dados sanitizados e me ajude a validar:
> 1. Os tipos de dados e os valores inválidos.
> 2. A definição correta do preço total registrado e do preço médio.
> 3. Três perguntas de negócio relevantes.
> 4. Os filtros necessários e como devem interagir.
>
> A dashboard será construída em um único arquivo HTML, com CSS e JavaScript, usando Chart.js para os gráficos. Quero uma interface profissional inspirada no visual da Porsche, com indicadores comerciais, gráficos interativos e filtros por modelo, estado, ano do modelo, método de pagamento e status.
>
> Não invente resultados. Não descarte registros sem justificar. Explique as decisões antes de gerar o código e depois me ajude a validar cada funcionalidade. A solução deverá ser compatível com o GitHub Pages e ter instruções para publicação e documentação no README.md.

## Como o prompt evoluiu

Durante o desenvolvimento, o pedido foi refinado com base na inspeção da base e nos testes:

1. Definiu-se que a soma deveria ser denominada **preço total registrado**, sem alegar que representava receita realizada.
2. Os registros foram preservados, inclusive os que possuem data `INVALID`; essas datas seriam excluídas somente de cálculos temporais.
3. As três perguntas foram fixadas em modelo, estado e método de pagamento, comparando valor e volume.
4. Os filtros foram definidos para atuar em conjunto e atualizar cartões e gráficos.
5. O ano do modelo foi convertido para número e a comparação do filtro foi ajustada.
6. O HTML foi limitado aos campos sanitizados necessários, sem nomes de clientes ou vendedores.
7. Foram incluídos rankings de modelos, dois rankings por estado e comparações por método de pagamento.
8. A imagem de destaque foi integrada ao cabeçalho sem adicionar um arquivo de imagem separado.

## Validação dos dados e dos filtros

A conferência da base durante o desenvolvimento registrou:

- 100 registros;
- 40 modelos distintos;
- 33 estados distintos;
- preço total registrado de **US$ 12.827.800,50**;
- 24 datas `INVALID` preservadas;
- comparação dos oito campos sanitizados incorporados ao HTML contra a planilha: nenhuma divergência identificada.

Os testes manuais reportados na revisão final passaram:

| Teste | Resultado esperado |
|---|---:|
| Carregamento inicial | 100 registros; US$ 12.827.800,50 |
| Modelo `911 Dakar` | 3 registros; US$ 810.600,00 |
| Estado `TX` | 14 registros; US$ 2.023.400,50 |
| Pagamento `Wire Transfer` | 26 registros; US$ 3.979.450,00 |
| Status `Delivered` | 41 registros; US$ 5.078.800,50 |

Na validação final no navegador, foram conferidos a imagem do Porsche, os quatro indicadores, os filtros, a atualização dos gráficos ao filtrar e o conteúdo deste README. Os resultados refletem a base sanitizada do exercício, não necessariamente vendas reais da Porsche.

## Estrutura do projeto

```text
porsche-sales-analytics/
├── index.html
└── README.md
```

## Entrega do desafio DIO

Para a submissão, informe principalmente o **link do repositório público**, e não somente o endereço da dashboard:

- Repositório: https://github.com/andrefelipegvt-art/porsche-sales-analytics
- Dashboard publicada: https://andrefelipegvt-art.github.io/porsche-sales-analytics/

Como evidência adicional para o portfólio, recomenda-se anexar à submissão uma captura da dashboard com pelo menos um filtro aplicado. Se a captura também for incluída no repositório, atualize a documentação para apontar para o caminho real do arquivo adicionado.

## Observações

- Projeto educacional com dados sanitizados fornecidos pela DIO.
- Todos os valores monetários são apresentados em USD.
- O preço total registrado não equivale necessariamente à receita realizada.
