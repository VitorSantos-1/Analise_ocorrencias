# Painel de Gestão e Operações do CPD

Relatório desenvolvido em **Power BI** para o acompanhamento operacional do **CPD (Centro de
Processamento / Distribuição)** de uma rede varejista. O painel dá visibilidade ao fluxo de
recebimento de mercadorias, à conferência de notas fiscais e à produtividade da equipe, apoiando
a gestão logística na identificação de gargalos e na tomada de decisão operacional.

---

## Visão Geral

O CPD é o ponto por onde entra toda a mercadoria da rede — e, quando ele trava, o efeito se
propaga para as lojas e para a venda. Esta solução consolida os registros de recebimento, as notas
de entrada e os dados operacionais em um modelo único, transformando a operação do centro de
distribuição em indicadores acompanháveis: volume processado, notas conferidas, tempo de
recebimento e divergências.

> **Nota de confidencialidade:** todos os dados exibidos no arquivo `.pbix` são fictícios e
> anonimizados, criados exclusivamente para demonstração de portfólio.

---

## Contexto de Negócio

A eficiência do recebimento determina a disponibilidade de produto na ponta. Atrasos na conferência,
divergências de quantidade e avarias na entrada geram ruptura de estoque, perdas financeiras e
retrabalho. Sem medição, esses problemas ficam invisíveis até virarem falta de produto na
prateleira ou prejuízo no inventário.

O painel torna a operação mensurável: mostra quanto o CPD processa por dia e por turno, quantas
notas foram conferidas e quantas estão pendentes, o tempo médio entre a chegada do veículo e a
liberação da mercadoria, e onde estão as divergências. Com isso, a gestão consegue priorizar
conferência, dimensionar equipe e cobrar SLA com base em dado.

## O Problema que Resolve

- **Falta de visibilidade** do volume e do status de recebimento em tempo próximo do real.
- **Gargalos ocultos** no fluxo de conferência de notas fiscais.
- **Divergências e avarias** de entrada não mapeadas, que viram perda no inventário.
- **Dimensionamento de equipe** feito sem base no volume operacional real.

## Público e Decisões Apoiadas

- **Gestão de logística / CPD:** prioriza conferência e ataca os gargalos de recebimento.
- **Prevenção de perdas:** identifica divergências e avarias na porta de entrada.
- **Planejamento operacional:** dimensiona equipe e turnos pelo volume real.

## Impacto e Valor Gerado

Trouxe visibilidade ao gargalo de recebimento (SLA, divergências e volume por turno), permitindo
priorizar a conferência, reduzir avaria e divergência na entrada e dimensionar a equipe pela demanda
real da operação — reduzindo ruptura e perda originadas no centro de distribuição.

---

## Arquitetura de Dados e Abordagem Técnica

### Fontes de dados
- Registros de **recebimento** (volumes / caixas processados por dia e turno).
- **Notas fiscais de entrada** (status de conferência).
- Dados operacionais de **logística** (tempos, divergências, equipes).

### Transformação (Power Query / M)
Consolidação dos registros de recebimento e das notas de entrada, padronização de datas e turnos,
cálculo de tempos entre eventos (chegada, início de conferência, liberação) e classificação de
divergências. O tratamento é centralizado na camada de consulta para manter o modelo consistente.

### Modelagem dimensional (Star Schema)
- **Fato Recebimento / Entrada** — grão por nota / volume recebido.
- **Dimensão Fornecedor** — origem da mercadoria.
- **Dimensão Equipe / Conferente** — responsável pela conferência.
- **Dimensão Calendário** — base das análises por dia, turno e período.

### Principais medidas (DAX) — lógica implementada

```DAX
Volume Processado = SUM( FatoEntrada[Volumes] )
NFs Conferidas    = CALCULATE( COUNTROWS( FatoEntrada ), FatoEntrada[Status] = "Conferida" )
NFs Pendentes     = CALCULATE( COUNTROWS( FatoEntrada ), FatoEntrada[Status] = "Pendente" )
SLA Medio (h)     = AVERAGEX( FatoEntrada, FatoEntrada[HoraLiberacao] - FatoEntrada[HoraChegada] )
% Divergencia     = DIVIDE( [Entradas com Divergencia], [Total de Entradas] )
```

> As fórmulas descrevem a lógica de cálculo do relatório e servem como referência de leitura do modelo.

---

## Indicadores e Análises (KPIs)

- **Volume de Entrada (caixas / volumes):** unidades físicas processadas por dia / turno.
- **Notas Fiscais Conferidas:** status de recepção de NF-e (conferidas versus pendentes).
- **SLA de Recebimento:** tempo médio entre a chegada do veículo e a liberação da mercadoria.
- **Divergências de Entrada:** inconsistências de quantidade ou avarias identificadas na recepção.

## Estrutura de Páginas do Relatório

1. **Visão Operacional** — volume processado, NFs conferidas / pendentes e SLA do dia.
2. **Produtividade** — desempenho por turno e por equipe.
3. **Divergências e Avarias** — mapeamento das inconsistências por fornecedor.

## Como Visualizar

O relatório é apresentado pelas imagens da seção **Prints** abaixo, que reproduzem as páginas
principais do painel. O arquivo-fonte (`.pbix`) não é versionado neste repositório por conter o
modelo de dados; pode ser disponibilizado sob solicitação para avaliação técnica.

## Estrutura de Arquivos

```text
README.md    -> Documentacao do projeto (contexto de negocio + arquitetura tecnica)
docs/        -> Prints das paginas do relatorio
.gitignore   -> Exclui binarios (.pbix) e planilhas de trabalho do versionamento
```

## Prints

As imagens abaixo apresentam as páginas principais do relatório — a imagem é o que comunica o
projeto de forma imediata. Os prints serão adicionados na pasta `docs/`.

<!-- Descomente ao adicionar a imagem:
![Visao operacional do CPD](docs/print-1.png)
-->

## Autor

Vitor Santos — Análise de Dados e Inteligência Comercial (Varejo).
