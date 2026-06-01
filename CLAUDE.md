# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Sobre o projeto

Análise da curva cota-área-volume da Barragem Algodões I (Cocal/PI), que rompeu em maio de 2009. Desenvolvido como trabalho da disciplina de Técnicas Hidrométricas do Mestrado Profissional em Gestão e Regulação de Recursos Hídricos (UFPI). A referência de validação é a dissertação de Sampaio (2014).

## Estrutura do repositório

```
reservatorios/
├── notebooks/trabalho_algodoes_gee.ipynb   ← análise principal
├── dados/                                  ← shapefiles (rastreados) e rasters (ignorados)
├── figuras/                                ← PNGs gerados (rastreados)
└── requirements.txt, CLAUDE.md, README.md, LICENSE
```

## Ambiente e execução

```powershell
# Ativar ambiente virtual (PowerShell)
.\.venv\Scripts\Activate.ps1

# Instalar dependências
pip install -r requirements.txt

# Configurar nbstripout (uma vez por clone — remove outputs do notebook antes de cada commit)
nbstripout --install --attributes .gitattributes

# Autenticar no Earth Engine (uma vez por máquina)
earthengine authenticate

# Abrir o notebook (sempre a partir da raiz do repositório)
jupyter notebook notebooks/trabalho_algodoes_gee.ipynb
```

O Python é 3.10.11. O GEE usa o projeto Google Cloud `meumapbiomas`.

## Arquivo principal

`notebooks/trabalho_algodoes_gee.ipynb` — execução sequencial obrigatória. As células dependem de variáveis definidas nas anteriores; não execute fora de ordem.

A célula 1 define três variáveis de caminho usadas pelo restante do notebook:
- `pasta_projeto` — raiz do repositório (Path absoluto, hardcoded)
- `pasta_dados` — `pasta_projeto / 'dados'` — onde o CSV gerado é salvo
- `pasta_figuras` — `pasta_projeto / 'figuras'` — onde os PNGs são salvos

## Fluxo da análise (células em ordem)

1. **Inicialização**: importa bibliotecas, inicializa GEE com projeto `meumapbiomas`, define caminhos.
2. **Área de estudo e DEM**: define polígono da área (coordenadas WGS84), carrega `COPERNICUS/DEM/GLO30`.
3. **Coleção Landsat 5**: filtra `LANDSAT/LT05/C02/T1_L2` de 1999-01-01 a 2009-05-01 com `CLOUD_COVER < 10`.
4. **Processamento das imagens**: função `processar_landsat5()` aplica fator de escala (`DN × 0.0000275 − 0.2`), mascara nuvens via bits do `QA_PIXEL`, calcula MNDWI = (B2−B5)/(B2+B5) e classifica água com limiar 0.
5. **Máscara histórica**: imagem de 2008-07-17 (maior área observada: 1,823 km²) gera a máscara da bacia hidráulica com buffer de 50 m.
6. **Calibração do DEM**: offset de +116,30 m calculado pela diferença entre a cota declarada na dissertação (314 m, jun/2008) e a cota máxima observada no DEM nos pixels de água (197,70 m).
7. **Curva CAV**: para cada cota de 280 a 322 m (passo 0,5 m), conta pixels do DEM corrigido abaixo do limiar dentro da máscara → área (km²); volume calculado por integração trapezoidal (hm³).
8. **Validação**: compara com Sampaio (2014): área 2,88 km² e volume 51,00 hm³ na cota 320 m.
9. **Saídas**: salva CSV e gráficos.

## Arquivos gerados pelo notebook

| Destino | Arquivo | Conteúdo |
|---|---|---|
| `dados/` | `curva_cota_area_volume_algodoes.csv` | Tabela com colunas `cota_m`, `area_km2`, `volume_hm3` — ignorado pelo git |
| `figuras/` | `curva_cota_area_volume_algodoes.png` | Três painéis: cota-área, cota-volume, área-volume |
| `figuras/` | `curva_cav_combinada_algodoes.png` | Gráfico único com dois eixos X para relatório |
| `dados/imagens_baixadas/` | `imagem_algodoes.tif` | GeoTIFF baixado via `geemap.ee_export_image` — ignorado pelo git |

## Dados de referência (Sampaio, 2014)

| Cota (m) | Área (km²) | Volume (hm³) |
|---|---|---|
| 320 (sangradouro) | 2,880 | 51,000 |
| 325 (coroamento) | — | — |

Cota do leito original estimada em ~285 m no referencial da dissertação.
