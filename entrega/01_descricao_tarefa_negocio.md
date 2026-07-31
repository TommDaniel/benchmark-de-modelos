# Descrição da Tarefa de Negócio

**Micro-capstone — Seleção de modelo de IA para produto SaaS B2B**

---

## 1. Contexto empresarial

Uma startup de software como serviço (SaaS) voltada ao mercado B2B deseja incorporar inteligência artificial a um módulo de análise textual de seu produto. O módulo será responsável por interpretar automaticamente tickets de suporte e feedbacks enviados por clientes, gerando classificações e recomendações que hoje dependem de trabalho manual da equipe de atendimento.

A escolha do modelo de IA é uma decisão estratégica, pois afeta diretamente:

- **Margem:** o custo por requisição é multiplicado pelo volume de tickets processados. Em um produto SaaS, que opera com receita recorrente e custo variável de infraestrutura, um modelo excessivamente caro pode corroer a margem de contribuição do módulo.
- **Experiência do usuário:** respostas lentas, classificações incorretas ou saídas mal formatadas degradam a percepção de qualidade do produto e podem gerar desconfiança na automação.
- **Escalabilidade:** o modelo precisa sustentar o crescimento da base de clientes sem que custo ou latência inviabilizem a operação em volumes de dezenas de milhares a milhões de requisições mensais.
- **Confiabilidade:** classificações erradas de urgência ou de risco de churn podem causar perda de clientes e danos à reputação; a saída precisa ser consistente e previsível.
- **Custo operacional:** além do custo direto de inferência, erros do modelo geram retrabalho humano, aumentando o custo operacional indireto.

## 2. Problema de decisão

A empresa precisa decidir qual dos três modelos candidatos oferece o melhor equilíbrio entre **desempenho técnico** (qualidade, consistência, adequação ao formato, latência) e **viabilidade econômica** (custo por requisição e em escala), considerando também o **risco empresarial** de automatizar decisões de suporte ao cliente.

Não se trata de escolher "o melhor modelo" em absoluto, e sim o modelo mais adequado ao caso de uso, ao volume esperado e ao apetite de risco da operação — incluindo a possibilidade de estratégias híbridas (por exemplo, roteamento por complexidade ou uso com revisão humana).

## 3. Tarefa selecionada

A tarefa empresarial escolhida para o benchmark é a **análise automatizada de tickets e feedbacks de clientes do SaaS B2B**.

Cada modelo avaliado receberá exatamente o mesmo texto de cliente (ticket ou feedback) e deverá produzir uma análise estruturada contendo:

- resumo do problema;
- categoria do ticket;
- sentimento;
- urgência;
- risco de churn;
- ação recomendada;
- tom sugerido para a resposta;
- justificativa da classificação.

### Aplicações práticas do caso de uso

Esse caso de uso pode ser utilizado para:

- **priorizar atendimentos**, ordenando a fila por urgência e criticidade;
- **encaminhar tickets** automaticamente para a equipe correta (suporte técnico, financeiro, comercial etc.);
- **detectar clientes com risco de cancelamento**, permitindo ação preventiva do time de customer success;
- **reduzir trabalho manual** de triagem e classificação;
- **melhorar o tempo de resposta**, acelerando a primeira resposta ao cliente;
- **apoiar atendentes humanos**, sugerindo tom, ação e contexto antes do atendimento.

## 4. Entrada esperada

Um ticket ou feedback textual escrito por um cliente, em linguagem natural, sem estrutura pré-definida (ex.: relato de erro, reclamação de cobrança, dúvida de uso, pedido de funcionalidade, ameaça de cancelamento).

## 5. Saída esperada

O modelo deve responder **exclusivamente** com um JSON válido na seguinte estrutura:

```json
{
  "resumo": "Resumo factual do problema",
  "categoria": "problema_tecnico | cobranca | integracao | usabilidade | solicitacao_de_recurso | comercial | seguranca | cancelamento | outro",
  "sentimento": "positivo | neutro | negativo",
  "urgencia": "baixa | media | alta | critica",
  "risco_churn": "baixo | medio | alto",
  "acao_recomendada": "Próxima ação recomendada",
  "tom_resposta": "acolhedor | consultivo | objetivo | urgente",
  "justificativa": "Justificativa curta baseada no texto"
}
```

## 6. Valor para o negócio

A automação dessa análise pode melhorar o suporte e a retenção de clientes de diversas formas:

- **Redução do tempo de triagem:** tickets passam a ser classificados em segundos, liberando a equipe para a resolução efetiva.
- **Priorização baseada em critérios objetivos:** clientes em situação crítica ou com risco de churn são atendidos primeiro, reduzindo cancelamentos evitáveis.
- **Padronização do atendimento:** a sugestão de tom e de ação recomendada reduz a variabilidade entre atendentes e melhora a consistência da experiência.
- **Geração de dados estruturados:** as classificações alimentam indicadores de saúde da base (categorias mais frequentes, sentimento médio, volume de tickets críticos), apoiando decisões de produto e de customer success.
- **Escalabilidade do suporte:** a operação cresce sem necessidade de aumento proporcional do time de triagem.

## 7. Riscos

A automação dessa tarefa envolve riscos que devem ser medidos e mitigados:

- **Classificação incorreta:** categoria errada encaminha o ticket para a equipe errada, atrasando a resolução.
- **Urgência subestimada:** um problema crítico tratado como baixa prioridade pode gerar indisponibilidade prolongada ou perda do cliente.
- **Falso risco de churn:** sinalizações incorretas desperdiçam esforço do time de customer success ou, no sentido oposto, deixam clientes em risco sem ação.
- **Alucinação:** o modelo pode inventar fatos não presentes no ticket (ex.: citar funcionalidades, valores ou prazos inexistentes).
- **Encaminhamento incorreto:** derivado de classificação errada, gera retrabalho e frustração do cliente.
- **Saída fora do formato:** respostas que não sejam JSON válido quebram integrações automatizadas e exigem tratamento de exceção.
- **Dependência excessiva de automação:** decisões sensíveis (ex.: cancelamento, segurança) tomadas sem supervisão humana podem gerar perdas financeiras e reputacionais.

## 8. Modelos avaliados

Os três modelos candidatos à seleção são:

1. **DeepSeek V4 Flash**;
2. **Kimi K2.7**;
3. **Kimi K3**.

> **Nota:** nesta etapa não são apresentadas características técnicas ou comerciais desses modelos (arquitetura, tamanho de contexto, preços, limites de taxa etc.), pois tais informações ainda não foram verificadas. Todos os dados relevantes serão coletados durante a execução do benchmark e registrados na tabela comparativa de métricas (`03_tabela_comparativa_metricas.csv`).
