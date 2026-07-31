# Benchmark de Seleção de Modelo de IA para SaaS B2B

Este repositório contém um micro-capstone de pós-graduação sobre seleção de modelo de IA generativa para um módulo de análise textual em um produto SaaS B2B.

## Objetivo

Comparar três modelos de IA na tarefa de análise automatizada de tickets e feedbacks de clientes, avaliando qualidade, custo, velocidade, consistência, adequação ao formato e risco empresarial.

## Modelos avaliados

- DeepSeek V4 Flash
- Kimi K2.7
- Kimi K3

## Tarefa de negócio

Análise automatizada de tickets e feedbacks de clientes, produzindo:

- resumo do problema;
- categoria do ticket;
- sentimento;
- urgência;
- risco de churn;
- ação recomendada;
- tom sugerido para a resposta;
- justificativa da classificação.

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

> **Nota:** a pasta `temp/` contém arquivos de trabalho local (fonte do relatório e script de geração do PDF) e está excluída do versionamento pelo `.gitignore`.

## Como usar este repositório

1. Leia `entrega/01_descricao_tarefa_negocio.md` para entender o contexto e a tarefa.
2. Consulte `entrega/02_plano_benchmark.md` para replicar ou ampliar o benchmark.
3. Use `prompt_benchmark.txt` como ponto de partida para os prompts nos modelos.
4. Veja `entrega/03_tabela_comparativa_metricas.csv` para os resultados numéricos.
5. Leia `entrega/04_relatorio_final.pdf` e `entrega/05_resumo_executivo_lideranca.md` para a análise e recomendação.

## Resultado em destaque

> **Recomendação:** adotar o DeepSeek V4 Flash (plano pago) como modelo principal, com Kimi K2.7 como fallback e revisão humana dos casos de alto risco. Kimi K3 não é recomendado como padrão neste momento devido ao custo e à latência elevados.
>
> Os dados completos, metodologia e ressalvas estão nos arquivos de entrega.

## Autor

TommDaniel — Daniel Felipe Tomm
