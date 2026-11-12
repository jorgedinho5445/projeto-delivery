# Ficha de Autoavaliação do Projeto Final

Este documento apresenta a reflexão crítica sobre o desenvolvimento do projeto de análise exploratória e higienização do dataset `delivery_pedidos.csv`, mapeando as competências técnicas adquiridas, os desafios operacionais superados e a correspondência com os objetivos de negócios estabelecidos para a operação de 2024.

---

## 1. Conclusões Baseadas nos Dados e Insights de Negócio

A análise dos dados reais de logística de entrega permitiu extrair métricas cruciais para a reestruturação tática da plataforma:

*   **Dominância Financeira Regional:** A praça de São Paulo demonstrou ser o motor econômico absoluto da operação, concentrando um faturamento acumulado de **R$ 20.620.069,95**. Esse patamar justifica o direcionamento prioritário de investimentos em marketing de retenção e expansão de malha de parceiros para a capital paulista.
*   **Gargalo de Atendimento e Resiliência:** O tempo médio geral de entrega da plataforma estabilizou-se em **35,9 minutos** após a remoção de anomalias sistêmicas. Identificou-se que em cenários de atraso severo (acima de 45 minutos), a nota média de satisfação do consumidor cai para **3,97 estrelas**. Embora o cliente demonstre resiliência relativa, médias abaixo de 4,0 indicam risco crítico de atrito e *churn* (perda de usuários para a concorrência).
*   **Sensibilidade Climática:** O cruzamento das variáveis de tempo com a flag de precipitação provou que os dias chuvosos afetam linearmente a velocidade do fluxo de entregas, exigindo o planejamento de incentivos financeiros dinâmicos para atrair maior volume de motociclistas parceiros sob condições adversas.

---

## 2. Processo de Tratamento de Dados (Data Cleaning) e Engenharia de Recursos

O maior ganho técnico deste projeto residiu na identificação de inconsistências estruturais e na aplicação de etapas de higienização automatizadas:

*   **Padronização de Strings Complexas:** O dataset apresentava problemas de duplicidade categórica, listando grupos redundantes devido a grafias inconsistentes como `SP`, `SAO PAULO`, `RIO` e `RJ`. A criação de uma função de mapeamento em dicionário acoplada ao `.str.upper().str.strip()` unificou as categorias nas três capitais de atuação oficiais.
*   **Correção de Tipos e Coerção Monetária:** A coluna `valor_pedido` continha strings que misturavam o caractere de cifrão (`R$`) com separadores de milhar e decimal fora do padrão numérico do Python. Desenvolveu-se uma função de limpeza via `.apply()` para remover os caracteres de texto e converter o campo em `float`, viabilizando somatórios e análises de ticket médio.
*   **Isolamento de Outliers e Regras de Negócio:** Tratamos registros corrompidos com tempos de entrega marcados como `999` minutos através da inserção de valores nulos (`np.nan`) e criamos uma coluna indicadora de erro (`tempo_invalido`). Adicionalmente, implementamos o conceito de segmentação estatística ao calcular o Percentil 90 (`.quantile(0.90)`) para sinalizar transações de alto valor corporativo através da flag `pedido_grande`.

---

## 3. Autoavaliação de Competências Técnicas

Abaixo consta a matriz de desempenho técnico com base na execução autônoma das etapas do projeto:

*   **Domínio de Manipulação com Pandas (Excelente):** Capacidade consolidada de carregar datasets, realizar agrupamentos complexos com `.groupby()` para extração de somas e médias direcionadas ao negócio.
*   **Resolução de Erros e Ambientes (Intermediário a Avançado):** Compreensão prática de caminhos de arquivos e resolução de exceções como `FileNotFoundError` e `ValueError` de dimensões de dados através do gerenciamento de pastas e refatoração de código dinâmico.
*   **Visualização de Dados Corporativa (Excelente):** Evolução de gráficos padrão para modelos visuais sofisticados e limpos (como o gráfico de rosca com legenda financeira externa e rótulos de valores diretamente sobre as barras), garantindo excelente legibilidade para apresentações executivas.

---
*Ficha preenchida e integrada ao repositório Git em outubro de 2026 como critério de conclusão do módulo.*
