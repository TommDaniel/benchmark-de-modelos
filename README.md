# Benchmark de Seleção de Modelo de IA para SaaS B2B

Este repositório contém um micro-capstone de pós-graduação sobre seleção de modelo de IA generativa para um módulo de análise textual em um produto SaaS B2B.

## Objetivo

O objetivo é comparar três modelos de IA na tarefa de análise automatizada de tickets e feedbacks de clientes. A comparação avalia qualidade, custo, velocidade, consistência, adequação ao formato e risco empresarial.

## Modelos avaliados

Os modelos avaliados são DeepSeek V4 Flash, Kimi K2.7 e Kimi K3.

## Tarefa de negócio

A tarefa é a análise automatizada de tickets e feedbacks de clientes. Cada modelo deve produzir resumo do problema, categoria do ticket, sentimento, urgência, risco de churn, ação recomendada, tom sugerido para a resposta e justificativa da classificação.

## Estrutura do repositório

```text
├── entrega/                           # Arquivos finais do trabalho
│   ├── 01_descricao_tarefa_negocio.md # Contexto, tarefa, riscos e modelos
│   ├── 02_plano_benchmark.md          # Plano de execução do benchmark
│   ├── 03_tabela_comparativa_metricas.csv # Métricas consolidadas
│   ├── 04_relatorio_final.pdf         # Relatório final em PDF
│   └── 05_resumo_executivo_lideranca.md # Resumo executivo
├── prompt_benchmark.txt               # Prompt padrão usado nos testes
└── README.md                          # Este arquivo
```

A pasta temp contém arquivos de trabalho local, como a fonte do relatório e o script de geração do PDF. Ela está excluída do versionamento pelo arquivo .gitignore.

## Como usar este repositório

Leia o arquivo entrega/01_descricao_tarefa_negocio.md para entender o contexto e a tarefa.

Consulte o arquivo entrega/02_plano_benchmark.md para replicar ou ampliar o benchmark.

Use o arquivo prompt_benchmark.txt como ponto de partida para os prompts nos modelos.

Veja o arquivo entrega/03_tabela_comparativa_metricas.csv para os resultados numéricos.

Leia os arquivos entrega/04_relatorio_final.pdf e entrega/05_resumo_executivo_lideranca.md para a análise completa e a recomendação.

## Resultado em destaque

A recomendação é adotar o DeepSeek V4 Flash no plano pago como modelo principal, com Kimi K2.7 como fallback e revisão humana dos casos de alto risco. O Kimi K3 não é recomendado como padrão neste momento devido ao custo elevado e à latência alta.

Os dados completos, a metodologia e as ressalvas estão nos arquivos de entrega.

## Autor

TommDaniel, Daniel Felipe Tomm.
