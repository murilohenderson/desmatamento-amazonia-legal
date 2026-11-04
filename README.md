# Desmatamento na Amazônia Legal por município, estado e região

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/murilohenderson/desmatamento-amazonia-legal/blob/main/notebooks/desmatamento_areas.ipynb)

Cálculo da área desmatada (km² e % da área) em cada um dos 773 municípios da Amazônia Legal, com agregação por estado e para a região como um todo, a partir dos dados do PRODES/INPE. O desmatamento é separado em dois períodos:

- **Histórico:** desmatamento acumulado até 2007, incluindo áreas desmatadas antes do início do monitoramento do PRODES (1988);
- **Recente:** incremento anual de 2008 a 2025.

## Resultados

Amazônia Legal: **1.116.143 km² desmatados (22,3% da área)**, sendo 17,9% até 2007 e 4,4% entre 2008 e 2025.

Municípios de estudo (Pará):

| Município | Área (km²) | Até 2007 (%) | 2008–2025 (%) | Total (km²) | Total (%) |
|---|---:|---:|---:|---:|---:|
| São Félix do Xingu | 84.191 | 18,4 | 7,3 | 21.641 | 25,7 |
| Altamira | 159.482 | 3,7 | 4,2 | 12.555 | 7,9 |
| Paragominas | 19.323 | 43,2 | 3,5 | 9.011 | 46,6 |
| Marabá | 15.054 | 50,9 | 8,8 | 8.976 | 59,6 |
| Itaituba | 62.051 | 7,1 | 4,5 | 7.165 | 11,6 |
| Santarém | 18.013 | 16,3 | 2,2 | 3.323 | 18,5 |
| Redenção | 3.824 | 74,1 | 5,6 | 3.049 | 79,7 |
| Canaã dos Carajás | 3.147 | 54,8 | 1,0 | 1.755 | 55,8 |
| Parauapebas | 6.957 | 18,2 | 1,4 | 1.363 | 19,6 |
| Cachoeira do Arari | 3.036 | 2,8 | 0,1 | 88 | 2,9 |

As tabelas completas (municípios, estados e Amazônia Legal) estão em [`resultados/desmatamento_areas.xlsx`](resultados/desmatamento_areas.xlsx).

## Dados

Os dados brutos não estão no repositório por causa do tamanho. Todos são públicos e podem ser baixados no [TerraBrasilis/INPE](https://terrabrasilis.dpi.inpe.br/downloads/):

| Camada | Uso |
|---|---|
| `municipalities_legal_amazon` | Limites municipais da Amazônia Legal |
| `states_legal_amazon` | Limites estaduais (dissolvidos para obter o limite da Amazônia Legal) |
| `yearly_deforestation_biome_amazonia` (v20261016) | Desmatamento anual 2008–2025, bioma Amazônia |
| `accumulated_deforestation_2007_biome_amazonia` (v20261016) | Desmatamento acumulado até 2007, bioma Amazônia |
| PRODES Cerrado (máscara até 2000, 2001–2007 e 2008–2025) | Desmatamento na porção da Amazônia Legal no bioma Cerrado, recortado por período |

## Método

1. Todas as camadas são reprojetadas para **South America Albers Equal Area (ESRI:102033)**, uma projeção de área igual, para que as áreas sejam calculadas em m² e não em graus.
2. Os municípios são recortados pelo limite da Amazônia Legal.
3. Para cada polígono de desmatamento, um índice espacial (STRtree) identifica os municípios que ele intersecta. Polígonos inteiramente contidos em um município entram com a área total; apenas os que cruzam divisas passam por interseção geométrica.
4. As áreas são somadas por município e, a partir dos municípios, por estado e para a Amazônia Legal, de forma que os três níveis sejam consistentes entre si.

As camadas do PRODES não se sobrepõem por construção (separação por bioma e por período), por isso não há dissolve entre elas.

**Observação:** os valores estaduais referem-se apenas à porção de cada estado dentro da Amazônia Legal; no caso do Maranhão, a parte fora da Amazônia Legal não entra no cálculo.

## Como rodar

1. Baixe as camadas listadas em **Dados** e envie para o seu Google Drive.
2. Abra o notebook no Colab pelo botão acima.
3. Ajuste os nomes dos arquivos e a lista de municípios de estudo na célula de configuração.
4. Execute todas as células. O processamento leva de 5 a 20 minutos no Colab gratuito.

Dependências: `geopandas`, `shapely>=2.0`, `pyogrio`, `pandas`, `openpyxl`.

## Como citar

Henderson, M. (2026). *Desmatamento na Amazônia Legal por município, estado e região (PRODES/INPE, acumulado até 2025)*. Versão 1.0. Disponível em: https://github.com/murilohenderson/desmatamento-amazonia-legal

O botão **Cite this repository**, na lateral do GitHub, gera a referência em outros formatos.

## Licença

Código sob [licença MIT](LICENSE). Tabelas e mapas gerados sob [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.pt-br). Os dados de origem pertencem ao INPE e ao IBGE e seguem as condições de uso de cada instituição.

## Autor

**Murilo Henderson** · Sistemas de Informação, UFRA
[GitHub](https://github.com/murilohenderson) · [LinkedIn](https://linkedin.com/in/murilohenderson)
