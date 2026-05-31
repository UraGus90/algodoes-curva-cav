# Análise da curva cota-área-volume da Barragem Algodões I

Estudo de caso utilizando sensoriamento remoto e Google Earth Engine para
construir a curva cota-área-volume da Barragem Algodões I, localizada no
município de Cocal, Piauí, que rompeu em 27 de maio de 2009.

## Contexto

Trabalho desenvolvido no âmbito do Mestrado Profissional em Gestão e
Regulação de Recursos Hídricos da Universidade Federal do Piauí (UFPI),
disciplina de Técnicas Hidrométricas.

## Metodologia

A análise combina três fontes de dados principais:

- **Imagens Landsat 5** (1999 a 2009): séries temporais do período de
  operação do reservatório original, usadas para identificar a extensão
  da bacia hidráulica em sua máxima ocupação histórica.

- **Modelo Digital de Elevação Copernicus GLO-30**: base topográfica para
  simulação do enchimento hipotético do vale. Aplicado offset de 116,30 m
  para calibração com referencial vertical da dissertação Sampaio (2014).

- **Dados estruturais da barragem**: parâmetros de projeto compilados na
  dissertação de Sampaio (2014), usados como referência de validação.

## Resultados principais

A simulação produz curva cota-área-volume completa entre cotas 280 e 322 m
no referencial da dissertação. Na cota do sangradouro (320 m):

- Área simulada: 2,343 km² (declarada na dissertação: 2,88 km²)
- Volume simulado: 41,934 hm³ (declarado: 51,00 hm³)

A descoberta relevante é que o reservatório aparentemente nunca atingiu
sua capacidade máxima durante os 10 anos de operação, com área máxima
histórica observada de 1,823 km² em 17 de julho de 2008.

## Estrutura do repositório

- `primeiro_mapa.ipynb`: notebook principal com toda a análise
- `curva_cota_area_volume_algodoes.csv`: tabela com a curva cota-área-volume
- `*.png`: gráficos gerados pela análise
- `*.shp` e `*.geojson`: contornos vetoriais do reservatório histórico

## Como reproduzir

1. Clone este repositório
2. Crie ambiente virtual Python 3.10 com `python -m venv .venv`
3. Ative o ambiente: `.\.venv\Scripts\Activate.ps1` (Windows PowerShell)
4. Instale dependências: `pip install earthengine-api geemap pandas matplotlib jupyter ipykernel geopandas`
5. Configure projeto Google Cloud para Earth Engine
6. Execute o notebook em ordem das células

## Referência principal

SAMPAIO, M. V. N. **Segurança de barragens de terra: um relato da
experiência do Piauí**. 2014. Dissertação (Mestrado em Engenharia Civil) -
Universidade Federal do Ceará, Fortaleza, 2014.

## Autor

Gustavo Guimarães Cruz

## Licença

MIT License
