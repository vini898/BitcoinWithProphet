# O Prophet agrega algo ao naive na previsão do preço do Bitcoin?

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/vini898/BitcoinWithProphet/blob/main/notebooks/Seminario_Bitcoin_Prophet_v10.ipynb)

Notebook de seminário que **replica, em dados atualizados, a avaliação de um tutorial de referência (RPubs) sobre o uso do Prophet para prever o BTC-USD**, corrigindo as falhas metodológicas apontadas na revisão desse tutorial: transformação log, benchmarks simples, validação de origem móvel e testes aplicados às séries corretas.

> **Aviso:** isto é uma replicação do *método* com outra janela de dados, e **não** uma reprodução dos números do tutorial. Os erros daqui não são comparáveis aos dele.

## Resultado em uma frase

Com este desenho, **não encontramos evidência de que o Prophet supere o naive**: na validação cruzada ele foi pior com significância estatística; no teste de 2025 foi pior em estimativa pontual, mas sem significância após correção para comparações múltiplas. Isso **não** prova que o preço do Bitcoin seja imprevisível. Veja a Etapa 8 do notebook, com os resultados, as limitações e o que a conclusão não afirma.

## Desenho

| Item | Valor |
|---|---|
| Ativo / fonte | BTC-USD, fechamento diário (Yahoo Finance via `yfinance`) |
| Período | 2017-01-01 a 2025-12-31 (3287 dias, sem faltantes) |
| Treino | 2922 dias (até 2024-12-31) |
| Teste | 365 dias (todo 2025) |
| Validação | origem móvel (janela expansiva) só no treino: 48 origens, horizonte de 30 dias |
| Teste A | 12 origens de 30 dias dentro de 2025 |
| Teste B | uma única trajetória de 365 dias (sem inferência) |
| Comparação | Diebold-Mariano com correção HLN e Holm, uma perda por origem |

Etapas do notebook: 1) dados e divisão treino/teste; 2) análise exploratória; 3) decomposição MSTL com controle por permutação; 4) benchmarks e Prophet com defaults; 5) validação de origem móvel; 6) avaliação no teste; 7) resíduos e intervalos de previsão; 8) conclusão.

## Como executar

**No Colab:** clique no botão acima. Se aparecer erro de numpy/binários após a instalação, use *Ambiente de execução > Reiniciar sessão*.

**Localmente:**

```bash
git clone https://github.com/vini898/BitcoinWithProphet.git
cd bitcoin-prophet-seminario
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab notebooks/Seminario_Bitcoin_Prophet_v10.ipynb
```

As células do Prophet na validação e nos intervalos (Células 18 e 26) **demoram**; o notebook imprime uma estimativa. Há `CV_MAX_ORIGENS` e `N_PERM` na Célula 1 para testes rápidos.

## Reprodutibilidade

- Seed fixa (`SEED = 42`), com `reseed()` antes de cada operação aleatória.
- Versões das bibliotecas em `requirements.txt` (as do ambiente em que o notebook foi executado).
- O Yahoo Finance pode revisar valores. Os números do notebook foram lidos de uma execução com dados baixados em **2026-10-03 (UTC)**, com **md5 `e90470b4ee76f9d2e9ffbd3eafebe2a2`** (Célula 3). Se uma nova execução gerar outro md5, os números precisam ser reconferidos.
- O notebook **não foi reexecutado do zero** depois das últimas edições de texto e rótulos. As saídas guardadas são as da execução acima (detalhes na primeira célula da Etapa 8).

## Estrutura

```
notebooks/   notebook principal (com as saídas guardadas)
data/        snapshot dos dados (btc_close.csv)
results/     CSVs gerados pelas Etapas 5, 6 e 7
```

## Licença

MIT. Veja [LICENSE](LICENSE).
