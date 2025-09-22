# Identificação do Perfil Ideal de Cliente

## Resumo
Este README explica, de forma simples, como chegamos ao perfil ideal de cliente a partir da nota (1–100) atribuída pela diretoria, usando a base clientes.csv e análise em Python/Plotly.

## Objetivo
Encontrar características (idade, profissão, renda, experiência, tamanho da família, etc.) que estejam associadas às notas mais altas, para identificar o cliente ideal que maximiza faturamento/lucro.

## Dados usados
Arquivo: clientes.csv (link fornecido)

## Passos realizados (resumido)

### Carregamento
- pandas.read_csv("clientes.csv", encoding="latin", sep=";")

### Limpeza
- Excluir coluna "Unnamed: 8".
- Converter "Salário Anual (R$)" para numérico: pd.to_numeric(..., errors="coerce").
- Remover linhas com NA: tabela = tabela.dropna().

### Análise descritiva
- tabela.describe() para estatísticas gerais (médias, desvios, min/max).

### Análise por característica
- Para cada coluna do dataset, plotamos um histograma/coluna com Plotly que mostra a média da "Nota (1-100)" por categoria/intervalo:
    - Código (resumo):
    ```
    for coluna in tabela.columns:
        grafico = px.histogram(tabela, x=coluna, y="Nota (1-100)", histfunc="avg", nbins=10)
        grafico.show()
    ```
- Interpretação: usamos a média da nota dentro de cada faixa/categoria para entender quais valores de cada variável estão associados a notas mais altas.

## Como foi derivado o perfil ideal
- Para cada variável (idade, profissão, renda, experiência, etc.), observou-se a faixa/categoria com maior média de nota. O perfil ideal foi construído combinando as faixas que apresentavam médias de nota mais elevadas em suas respectivas análises univariadas.
- Observações relevantes encontradas:
    - Idade: clientes com mais de 15 anos de idade apresentaram notas consistentemente mais altas; diferenças entre faixas acima disso foram pequenas.
    - Profissão/Área: melhores médias nas áreas “Entretenimento” e “Artista”; menor desempenho em “Construção”.
    - Experiência: faixa de 10–15 anos de experiência mostrou maior média de nota.
    - Tamanho da família: famílias pequenas a médias (até ~7 pessoas) associadas a notas melhores.

## Perfil ideal (síntese)
- Idade: > 15 anos (sem diferenças grandes entre faixas superiores)
- Profissão: Entretenimento / Artista (evitar Construção)
- Experiência: 10–15 anos
- Tamanho da família: até 7 pessoas

## Reprodutibilidade — Código mínimo para reproduzir
(Sintetizado)

```
import pandas as pd
import plotly.express as px

tabela = pd.read_csv("clientes.csv", encoding="latin", sep=";")
tabela = tabela.drop("Unnamed: 8", axis=1)
tabela["Salário Anual (R$)"] = pd.to_numeric(tabela["Salário Anual (R$)"], errors="coerce")
tabela = tabela.dropna()

print(tabela.info())
display(tabela.describe())

for coluna in tabela.columns:
    grafico = px.histogram(tabela, x=coluna, y="Nota (1-100)", histfunc="avg", text_auto=True, nbins=10)
    grafico.show()
```
