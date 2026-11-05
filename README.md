# Desmatamento na Amazônia Legal por município, estado e região

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23959180.svg)](https://doi.org/10.5281/zenodo.23959180)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/murilohenderson/desmatamento-amazonia-legal/blob/main/notebooks/desmatamento_areas.ipynb)

Cálculo da área desmatada (km² e % da área) em cada um dos 773 municípios da Amazônia Legal, com agregação por estado e para a região como um todo, a partir dos dados do PRODES/INPE. O desmatamento é separado em dois períodos:

- **Histórico:** desmatamento acumulado até 2007, incluindo áreas desmatadas antes do início do monitoramento do PRODES (1988);
- **Recente:** incremento anual de 2008 a 2025.

## Resultados

Amazônia Legal: **1.116.143 km² desmatados (22,3% da área)**, sendo 17,9% até 2007 e 4,4% entre 2008 e 2025.

Por estado (porção dentro da Amazônia Legal):

| UF | Área (km²) | Até 2007 (%) | 2008–2025 (%) | Total (km²) | Total (%) |
|---|---:|---:|---:|---:|---:|
| MT | 903.062 | 35,1 | 5,1 | 363.572 | 40,3 |
| PA | 1.244.912 | 19,1 | 4,7 | 297.204 | 23,9 |
| MA | 260.510 | 46,0 | 11,0 | 148.363 | 57,0 |
| TO | 277.190 | 27,4 | 13,3 | 112.680 | 40,6 |
| RO | 237.383 | 34,9 | 7,2 | 99.822 | 42,0 |
| AM | 1.558.265 | 2,1 | 1,2 | 51.188 | 3,3 |
| AC | 163.908 | 11,8 | 4,6 | 26.911 | 16,4 |
| RR | 223.001 | 3,8 | 2,1 | 13.168 | 5,9 |
| AP | 141.387 | 1,9 | 0,4 | 3.235 | 2,3 |

Os 10 municípios com maior área desmatada:

| Município | UF | Área (km²) | Total (km²) | Total (%) |
|---|---|---:|---:|---:|
| São Félix do Xingu | PA | 84.191 | 21.641 | 25,7 |
| Altamira | PA | 159.482 | 12.555 | 7,9 |
| Porto Velho | RO | 34.069 | 12.553 | 36,9 |
| Paranatinga | MT | 24.183 | 9.705 | 40,1 |
| Paragominas | PA | 19.323 | 9.011 | 46,6 |
| Marabá | PA | 15.054 | 8.976 | 59,6 |
| Novo Repartimento | PA | 15.329 | 8.926 | 58,2 |
| Juara | MT | 22.751 | 8.596 | 37,8 |
| Novo Progresso | PA | 38.179 | 7.775 | 20,4 |
| Santana do Araguaia | PA | 11.601 | 7.668 | 66,1 |

## Arquivos gerados

Na pasta [`resultados/`](resultados/), exceto o GeoPackage:

| Arquivo | Conteúdo |
|---|---|
| `desmatamento_areas.xlsx` | Tabelas por município, estado e Amazônia Legal, e série anual 2008–2025 nos três níveis |
| `municipios.csv` | Área e desmatamento (km² e %) dos 773 municípios |
| `estados.csv` | Área e desmatamento (km² e %) por estado (porção na Amazônia Legal) |
| `serie_anual_municipios.csv` | Desmatamento anual por município, 2008–2025, em formato longo (município × ano) |
| `desmatamento_amazonia_legal.gpkg` | Disponível nos arquivos da [release](https://github.com/murilohenderson/desmatamento-amazonia-legal/releases/latest) e no Zenodo. Camadas `municipios` e `estados` com geometria (SIRGAS 2000) e todos os indicadores, incluindo a série anual (`a2008` a `a2025`) |

Nos CSV e no GeoPackage, as colunas seguem o padrão `historico_ate_2007_km2`, `recente_2008_2025_km2`, `total_km2` e os respectivos `_pct`.

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
4. No período 2008–2025, as áreas são somadas também por ano, a partir do campo de ano das camadas do PRODES, o que gera a série anual por município.
5. As áreas são somadas por município e, a partir dos municípios, por estado e para a Amazônia Legal, de forma que os três níveis sejam consistentes entre si.

As camadas do PRODES não se sobrepõem por construção (separação por bioma e por período), por isso não há dissolve entre elas.

**Observação:** os valores estaduais referem-se apenas à porção de cada estado dentro da Amazônia Legal; no caso do Maranhão, a parte fora da Amazônia Legal não entra no cálculo.

## Validação

Os resultados foram comparados com valores oficiais publicados:

| Recorte | Fonte oficial | Oficial | Este trabalho | Diferença |
|---|---|---:|---:|---:|
| Acre, desmatamento acumulado até 2025 | [SEMA-AC / PRODES](https://sema.ac.gov.br/wp-content/uploads/2026/04/Relatorio-Desmatamento-PRODES-2024_2025-1.pdf) | 26.918,8 km² (16,4%) | 26.911,0 km² (16,4%) | −0,03% |
| Acre, 2008–2025 (soma das taxas anuais) | SEMA-AC / PRODES | 7.623 km² | 7.505 km² | −1,5% |

O Acre está inteiramente no bioma Amazônia, o que permite testar as camadas do PRODES Amazônia e o recorte municipal sem interferência do Cerrado. A diferença no período 2008–2025 é esperada: as taxas oficiais incluem uma correção estatística para áreas sob nuvem, enquanto este trabalho soma os polígonos efetivamente mapeados.

A série anual do Acre também confere com os incrementos mapeados citados no mesmo relatório: 1.006 km² em 2022 e 275 km² em 2025.

**Limitações:**

- **Série anual no Cerrado:** até 2012, o PRODES Cerrado é bienal. Por isso, na porção de Cerrado (sobretudo MT, TO e MA), os valores de 2008, 2010 e 2012 acumulam dois anos, e os de 2009 e 2011 ficam próximos de zero. Para análises anuais nesses estados, use a série a partir de 2013 ou agrupe 2008–2012 em biênios.
- Nos estados com grande porção de Cerrado (MT, TO e MA), os valores dependem principalmente das camadas do PRODES Cerrado e não foram comparados com totais oficiais.
- Os valores por município podem ser conferidos no [painel do PRODES no TerraBrasilis](https://terrabrasilis.dpi.inpe.br/app/dashboard/deforestation/biomes/legal_amazon/rates).

## Como rodar

1. Baixe as camadas listadas em **Dados** e envie para o seu Google Drive.
2. Abra o notebook no Colab pelo botão acima.
3. Ajuste os nomes dos arquivos na célula de configuração, se necessário.
4. Execute todas as células. O processamento leva de 10 a 20 minutos no Colab gratuito. Os resultados são salvos em um `.zip` no Drive.

Dependências: `geopandas`, `shapely>=2.0`, `pyogrio`, `pandas`, `openpyxl`.

## Como citar

Costa, M. H. (2026). *Desmatamento na Amazônia Legal por município, estado e região (PRODES/INPE, acumulado até 2025)*. Versão 1.1. Zenodo, 2026. DOI: [10.5281/zenodo.23959180](https://doi.org/10.5281/zenodo.23959180).

Em ABNT:

> COSTA, Murilo Henderson. *Desmatamento na Amazônia Legal por município, estado e região (PRODES/INPE, acumulado até 2025)*. Versão 1.1. [S. l.]: Zenodo, 2026. DOI: 10.5281/zenodo.23959180.

O botão **Cite this repository**, na lateral do GitHub, gera a referência em outros formatos.

## Licença

Código sob [licença MIT](LICENSE). Tabelas e mapas gerados sob [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.pt-br). Os dados de origem pertencem ao INPE e ao IBGE e seguem as condições de uso de cada instituição.

## Autor

**Murilo Henderson Costa** · Sistemas de Informação, UFRA
[GitHub](https://github.com/murilohenderson) · [LinkedIn](https://linkedin.com/in/murilohenderson) · [Lattes](http://lattes.cnpq.br/8856926594323721)
