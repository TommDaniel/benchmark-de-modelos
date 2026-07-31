# Resumo Executivo para Liderança

**Seleção de modelo de IA para o módulo de análise textual — SaaS B2B**

---

## Decisão em análise

Qual modelo de IA deve ser adotado para o módulo de análise textual do produto, responsável por interpretar automaticamente tickets e feedbacks de clientes (resumo, categoria, sentimento, urgência, risco de churn, ação recomendada e tom de resposta).

## Alternativas avaliadas

- DeepSeek V4 Flash;
- Kimi K2.7;
- Kimi K3.

## Critérios principais

- **Qualidade** da análise e das classificações;
- **Custo** por requisição e em escala (10 mil, 100 mil e 1 milhão de requisições);
- **Latência**;
- **Consistência** entre execuções;
- **Adequação ao formato** (JSON válido, campos obrigatórios, valores permitidos);
- **Risco empresarial** (erros críticos, alucinação, segurança para automação).

## Principais resultados

Testes executados na plataforma OpenCode (5 tickets × 3 modelos × 3 execuções em lote por modelo = 10 execuções, 9 válidas). O DeepSeek V4 Flash teve 1 execução descartada (instabilidade de rede) e 3 válidas (1 na camada gratuita + 2 no plano pago, sem o teto de 200 requisições/dia).

| Métrica | DeepSeek V4 Flash | Kimi K2.7 | Kimi K3 |
|---------|-------------------|-----------|---------|
| Qualidade média (1 a 5) | 5,0 | 4,0 | 4,5 |
| Acurácia combinada média (%) | 93,3 | 75 | 83,3 |
| Custo médio por execução, lote de 5 tickets (USD) | 0,00109884 | 0,01351936 | 0,07587480 |
| Custo em 1 mi de execuções (USD) | 1.098,84 | 13.519,36 | 75.874,80 |
| Latência média (ms) | 17.119 | 13.212 | 110.128 |
| Consistência entre rodadas (%) | 40 | 60 | 60 |
| JSON válido no formato exigido (%) | 33,3* | 66,7 | 100 |
| Risco percebido (1 a 5) | 2 | 2 | 2 |
| Pontuação final (0 a 100) | 87,4 | 80,7 | 53,0 |
| Aprovado nos critérios eliminatórios | Não* | Não | Sim |

\* O DeepSeek respondeu os 5 tickets em JSON válido e completo em todas as rodadas, mas 2 de 3 vieram em bloco de código markdown e a rodada conforme usou envelope de objeto por ticket em vez de array — desvios de apresentação mitigáveis (parser tolerante ou exemplos de saída no prompt), que o reprovaram apenas no critério de formato. O Kimi K2.7 cometeu o desvio de bloco de código em 1 de 3 rodadas.

## Recomendação executiva

**Adotar o DeepSeek V4 Flash (plano pago) como modelo principal, com Kimi K2.7 como fallback e revisão humana dos casos de alto risco.**

- **Modelo principal:** DeepSeek V4 Flash no plano pago — melhor pontuação (87,4), melhor qualidade média do benchmark (93,3% de acurácia combinada), custo de US$ 0,00022 por ticket sem teto de uso e latência adequada (~17 s). Condições: mitigar os desvios de formato e melhorar a consistência (40%) com exemplos de calibragem de urgência e churn no prompt, confirmando em rodada ampliada. Para até 200 requisições/dia, a camada gratuita pode ser usada sem custo.
- **Fallback:** Kimi K2.7 — baixo custo e a menor latência média; requer calibragem de urgência (superestimada recorrentemente) e atenção ao formato (fence em 1/3 das rodadas).
- **Não recomendado como padrão neste momento:** Kimi K3 — único com formato perfeito, mas qualidade média inferior à do DeepSeek (83,3% vs. 93,3%), custo 69× maior e latência de 81–148 s; reavaliar como camada premium para casos críticos apenas se rodadas futuras confirmarem vantagem.
- **Revisão humana obrigatória** para tickets com urgência alta/crítica ou risco de churn médio/alto — em especial os perfis de cobrança e usabilidade ambígua, cujas classificações variaram em todos os modelos.

## Impacto esperado

- **Impacto na margem:** com o DeepSeek V4 Flash no plano pago, o custo projetado é de ~US$ 1,1 mil por milhão de execuções em lote (5 milhões de tickets) — cerca de 12× mais barato que o fallback (K2.7: ~US$ 13,5 mil) e 69× mais barato que o K3 (~US$ 75,9 mil); para até 200 requisições/dia, a camada gratuita zera o custo.
- **Impacto na experiência do usuário:** triagem automatizada em ~13–17 s por lote com os modelos principal e fallback, adequada a processamento assíncrono; o Kimi K3 (~110 s em média) não é adequado a tempo real.
- **Capacidade de escala:** o plano pago do modelo principal não tem o teto de 200 requisições/dia da camada gratuita e sustenta as projeções de volume a custo marginal baixo; o fallback pago garante contingência.
- **Risco operacional:** nenhum modelo apresentou erros críticos ou alucinações; os riscos residuais são os desvios de formato (DeepSeek em 2/3 rodadas; K2.7 em 1/3), a consistência limitada de todos os modelos nos tickets 2 e 4, e a superestimação de urgência do K2.7 — todos cobertos por revisão humana e calibragem de prompt.
- **Necessidade de supervisão humana:** sim — automação com revisão dos casos sinalizados como alto risco; não se recomenda automação completa neste estágio.
