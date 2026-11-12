# Chuva e atrasos acima de 45 minutos derrubam a satisfação do usuário: diagnóstico de eficiência do delivery em 2024

> A chuva adiciona em média 9,1 minutos ao tempo de entrega e atrasos severos derrubam a avaliação média para 3,97 estrelas; a empresa deve implementar tarifa dinâmica climática e limitar raios longos nos horários de pico em São Paulo.

![Figura principal](saidas/figura_principal.png)

*Painel comparativo do tempo médio de entrega (min) em dias normais versus operação sob chuva, destacando o gargalo operacional em 8.058 pedidos de 2024.*

---

## 1. Pergunta de negócio

A diretoria do aplicativo de delivery solicitou esta análise exploratória para embasar o planejamento estratégico e operacional de 2025. O estudo investigou três frentes principais: eficiência da operação logística (tempos e distâncias), comportamento e satisfação dos clientes (avaliações e ticket médio) e o impacto de alavancas comerciais e externas (uso de cupons e dias chuvosos).

**Perguntas que guiaram a análise:**
1. Em que horários e dias da semana se concentram os maiores volumes de pedidos?
2. Qual é o impacto real da chuva no tempo médio de entrega das operações?
3. Entregas demoradas (acima de 45 minutos) derrubam a nota média de avaliação dos clientes?
4. O uso de cupons de desconto eleva o ticket médio dos pedidos ou apenas reduz a margem?
5. Como o faturamento e a eficiência operacional se distribuem entre São Paulo, Rio de Janeiro e Belo Horizonte?

## 2. Dados

- **Fonte:** `delivery_pedidos.csv`, fornecida no módulo DS-PY-018 (dados simulados de um app de delivery).
- **Período e escopo:** 01/01/2024 a 31/12/2024; operações em São Paulo, Rio de Janeiro e Belo Horizonte.
- **Tamanho:** 8.058 linhas × 18 colunas (base original) → 8.058 linhas × 20 colunas (base limpa com adição de 2 flags).
- **Unidade de observação:** Um pedido individual realizado no aplicativo.

### Dicionário da base limpa (`saidas/delivery_pedidos_limpo.csv`)

| Coluna | Tipo | Descrição | % faltantes | Observação / tratamento |
|---|---|---|---|---|
| `pedido_id` | int64 | Identificador único do pedido | 0.0% | Chave primária mantida sem alterações. |
| `data_hora` | str | Data e hora da realização do pedido | 0.0% | Padronizada para ordenação temporal. |
| `cidade` | str | Cidade onde o pedido foi entregue | 0.0% | Variantes (`SP`, `SAO PAULO`, `BH`, `RJ`) padronizadas para 3 capitais. |
| `bairro` | str | Bairro de destino da entrega | 0.0% | Mantido conforme registro original. |
| `restaurante_id` | str | Identificador único do estabelecimento | 0.0% | Chave do parceiro comercial. |
| `tipo_cozinha` | str | Categoria gastronômica do pedido | 0.0% | Usada para comparação de ticket médio. |
| `plataforma` | str | Sistema operacional utilizado pelo cliente | 1.9% | Valores ausentes mantidos como nulos. |
| `forma_pagamento` | str | Meio de pagamento escolhido no checkout | 0.0% | Distribuição homogênea entre as praças. |
| `cupom` | str | Código ou categoria de desconto aplicado | 0.0% | Indica o uso de incentivo promocional. |
| `cliente_novo` | bool | Indica se é a primeira compra do usuário | 0.0% | Variável booleana (`True` ou `False`). |
| `chuva` | bool | Indica se chovia no momento do pedido | 0.0% | Variável booleana (`True` ou `False`). |
| `distancia_km` | float64 | Distância em km entre restaurante e cliente | 3.9% | Mantidos valores reais; correlação positiva com tempo. |
| `tempo_entrega_min` | float64 | Minutos decorridos do pedido até a entrega | 0.4% | Códigos de erro `999` e valores negativos convertidos para `NaN`. |
| `valor_pedido` | float64 | Valor dos itens em R$, sem taxa de entrega | 0.0% | Strings com cifrão (`R$ 44,02`) limpas e convertidas para float. |
| `taxa_entrega` | float64 | Valor cobrado pelo frete em R$ | 0.0% | Mantido conforme base original. |
| `avaliacao` | float64 | Nota dada pelo cliente após a entrega (1 a 5) | 16.7% | Valores ausentes mantidos como `NaN` para não enviesar a média. |
| `pais` | str | País da operação | 0.0% | Coluna constante (Brasil). |
| `moeda` | str | Moeda corrente da transação | 0.0% | Coluna constante (BRL). |
| `tempo_invalido` | bool | **Flag** indicando falha de registro temporal | 0.0% | `True` quando o tempo original registrado era `999` ou negativo. |
| `pedido_grande` | bool | **Flag** indicando pedidos de alto valor | 0.0% | `True` para pedidos acima do Percentil 90 (top 10% mais caros). |

## 3. Como rodar

```bash
git clone https://github.com
cd projeto-delivery
python -m venv .venv && source .venv/bin/activate     # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook projeto_final.ipynb
```

- O notebook lê o CSV original localizado em `dados/delivery_pedidos.csv` (caso não esteja no repositório, coloque o arquivo original dentro da pasta `dados/`).
- Execute **Kernel → Restart & Run All**. Tempo aproximado de execução: 15 segundos.
- Versões usadas: Python 3.10+, pandas 2.0+, seaborn 0.12+, matplotlib 3.7+, scipy 1.10+ (ver `requirements.txt`).

## 4. Estrutura do repositório

