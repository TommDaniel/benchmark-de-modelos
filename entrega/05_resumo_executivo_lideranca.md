# Resumo Executivo para Liderança

Seleção de modelo de IA para o módulo de análise textual em produto SaaS B2B.

## Decisão em análise

A decisão em análise é qual modelo de IA deve ser adotado para o módulo de análise textual do produto. Esse módulo é responsável por interpretar automaticamente tickets e feedbacks de clientes, gerando resumo, categoria, sentimento, urgência, risco de churn, ação recomendada e tom de resposta.

## Alternativas avaliadas

As alternativas avaliadas são DeepSeek V4 Flash, Kimi K2.7 e Kimi K3.

## Critérios principais

Os critérios principais de avaliação são a qualidade da análise e das classificações, o custo por requisição e em escala para 10 mil, 100 mil e 1 milhão de requisições, a latência, a consistência entre execuções, a adequação ao formato com JSON válido e campos obrigatórios, e o risco empresarial, incluindo erros críticos, alucinação e segurança para automação.

## Principais resultados

Os testes foram executados na plataforma OpenCode com 5 tickets, 3 modelos e 3 execuções em lote por modelo, totalizando 10 execuções, das quais 9 foram válidas. O DeepSeek V4 Flash teve 1 execução descartada por instabilidade de rede e 3 válidas, sendo 1 na camada gratuita e 2 no plano pago, sem o teto de 200 requisições por dia.

A tabela a seguir apresenta as principais métricas dos três modelos.

| Métrica | DeepSeek V4 Flash | Kimi K2.7 | Kimi K3 |
|---------|-------------------|-----------|---------|
| Qualidade média (1 a 5) | 5,0 | 4,0 | 4,5 |
| Acurácia combinada média (%) | 93,3 | 75 | 83,3 |
| Custo médio por execução, lote de 5 tickets (USD) | 0,00109884 | 0,01351936 | 0,07587480 |
| Custo em 1 mi de execuções (USD) | 1.098,84 | 13.519,36 | 75.874,80 |
| Latência média (ms) | 17.119 | 13.212 | 110.128 |
| Consistência entre rodadas (%) | 40 | 60 | 60 |
| JSON válido no formato exigido (%) | 33,3 | 66,7 | 100 |
| Risco percebido (1 a 5) | 2 | 2 | 2 |
| Pontuação final (0 a 100) | 87,4 | 80,7 | 53,0 |
| Aprovado nos critérios eliminatórios | Não | Não | Sim |

O DeepSeek respondeu os 5 tickets em JSON válido e completo em todas as rodadas, mas 2 de 3 vieram em bloco de código markdown e a rodada conforme usou envelope de objeto por ticket em vez de array. Esses desvios de apresentação são mitigáveis com parser tolerante ou exemplos de saída no prompt, e o reprovaram apenas no critério de formato. O Kimi K2.7 cometeu o desvio de bloco de código em 1 de 3 rodadas.

## Recomendação executiva

A recomendação é adotar o DeepSeek V4 Flash no plano pago como modelo principal, com Kimi K2.7 como fallback e revisão humana dos casos de alto risco.

O modelo principal é o DeepSeek V4 Flash no plano pago. Ele apresentou a melhor pontuação, 87,4, a melhor qualidade média do benchmark, 93,3% de acurácia combinada, custo de 0,00022 dólar por ticket sem teto de uso e latência adequada, de aproximadamente 17 segundos. As condições para adoção são mitigar os desvios de formato e melhorar a consistência, que foi de 40%, com exemplos de calibragem de urgência e churn no prompt, confirmando em rodada ampliada. Para até 200 requisições por dia, a camada gratuita pode ser usada sem custo.

O fallback é o Kimi K2.7. Ele apresenta baixo custo e a menor latência média, mas requer calibragem de urgência, que foi superestimada recorrentemente, e atenção ao formato, com fence em 1 de 3 rodadas.

O Kimi K3 não é recomendado como padrão neste momento. Ele é o único com formato perfeito, mas apresenta qualidade média inferior à do DeepSeek, 83,3% contra 93,3%, custo 69 vezes maior e latência de 81 a 148 segundos. Pode ser reavaliado como camada premium para casos críticos apenas se rodadas futuras confirmarem vantagem.

A revisão humana é obrigatória para tickets com urgência alta ou crítica, ou risco de churn médio ou alto, em especial os perfis de cobrança e usabilidade ambígua, cujas classificações variaram em todos os modelos.

## Impacto esperado

O impacto na margem é significativo com o DeepSeek V4 Flash no plano pago. O custo projetado é de aproximadamente 1,1 mil dólar por milhão de execuções em lote, equivalente a 5 milhões de tickets. Esse valor é cerca de 12 vezes mais barato que o fallback, que custa 13,5 mil dólares, e 69 vezes mais barato que o Kimi K3, que custa 75,9 mil dólares. Para até 200 requisições por dia, a camada gratuita zera o custo.

O impacto na experiência do usuário é positivo com a triagem automatizada em aproximadamente 13 a 17 segundos por lote com os modelos principal e fallback, adequada a processamento assíncrono. O Kimi K3, com 110 segundos em média, não é adequado a tempo real.

A capacidade de escala é sustentada pelo plano pago do modelo principal, que não tem o teto de 200 requisições por dia da camada gratuita e suporta as projeções de volume a custo marginal baixo. O fallback pago garante contingência.

O risco operacional é controlado. Nenhum modelo apresentou erros críticos ou alucinações. Os riscos residuais são os desvios de formato do DeepSeek em 2 de 3 rodadas e do Kimi K2.7 em 1 de 3, a consistência limitada de todos os modelos nos tickets 2 e 4, e a superestimação de urgência do Kimi K2.7. Todos esses riscos são cobertos por revisão humana e calibragem do prompt.

A supervisão humana é necessária. A automação deve incluir revisão dos casos sinalizados como alto risco, e não se recomenda automação completa neste estágio.
