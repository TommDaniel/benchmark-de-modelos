# Descrição da Tarefa de Negócio

Micro-capstone de seleção de modelo de IA para produto SaaS B2B.

## 1. Contexto empresarial

Uma startup de software como serviço voltada ao mercado B2B deseja incorporar inteligência artificial a um módulo de análise textual de seu produto. O módulo interpreta automaticamente tickets de suporte e feedbacks enviados por clientes, gerando classificações e recomendações que hoje dependem de trabalho manual da equipe de atendimento.

A escolha do modelo de IA é uma decisão estratégica, pois afeta diretamente a margem, a experiência do usuário, a escalabilidade, a confiabilidade e o custo operacional.

A margem é afetada porque o custo por requisição se multiplica pelo volume de tickets processados. Em um produto SaaS, que opera com receita recorrente e custo variável de infraestrutura, um modelo excessivamente carro pode corroer a margem de contribuição do módulo.

A experiência do usuário é afetada porque respostas lentas, classificações incorretas ou saídas mal formatadas degradam a percepção de qualidade do produto e podem gerar desconfiança na automação.

A escalabilidade é afetada porque o modelo precisa sustentar o crescimento da base de clientes sem que custo ou latência inviabilizem a operação em volumes de dezenas de milhares a milhões de requisições mensais.

A confiabilidade é afetada porque classificações erradas de urgência ou de risco de churn podem causar perda de clientes e danos à reputação. A saída precisa ser consistente e previsível.

O custo operacional é afetado porque, além do custo direto de inferência, erros do modelo geram retrabalho humano, aumentando o custo operacional indireto.

## 2. Problema de decisão

A empresa precisa decidir qual dos três modelos candidatos oferece o melhor equilíbrio entre desempenho técnico e viabilidade econômica, considerando também o risco empresarial de automatizar decisões de suporte ao cliente.

Não se trata de escolher o melhor modelo em absoluto, e sim o modelo mais adequado ao caso de uso, ao volume esperado e ao apetite de risco da operação. A decisão pode incluir estratégias híbridas, como roteamento por complexidade ou uso com revisão humana.

## 3. Tarefa selecionada

A tarefa empresarial escolhida para o benchmark é a análise automatizada de tickets e feedbacks de clientes do SaaS B2B.

Cada modelo avaliado receberá exatamente o mesmo texto de cliente, seja ticket ou feedback, e deverá produzir uma análise estruturada contendo resumo do problema, categoria do ticket, sentimento, urgência, risco de churn, ação recomendada, tom sugerido para a resposta e justificativa da classificação.

### Aplicações práticas do caso de uso

Esse caso de uso pode ser utilizado para priorizar atendimentos, ordenando a fila por urgência e criticidade. Pode ser utilizado para encaminhar tickets automaticamente para a equipe correta, como suporte técnico, financeiro ou comercial. Pode ser utilizado para detectar clientes com risco de cancelamento, permitindo ação preventiva do time de customer success. Pode reduzir o trabalho manual de triagem e classificação. Pode melhorar o tempo de resposta, acelerando a primeira resposta ao cliente. Pode apoiar atendentes humanos, sugerindo tom, ação e contexto antes do atendimento.

## 4. Entrada esperada

A entrada é um ticket ou feedback textual escrito por um cliente, em linguagem natural, sem estrutura pré-definida. Exemplos incluem relato de erro, reclamação de cobrança, dúvida de uso, pedido de funcionalidade ou ameaça de cancelamento.

## 5. Saída esperada

O modelo deve responder exclusivamente com um JSON válido na seguinte estrutura:

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

A automação dessa análise pode melhorar o suporte e a retenção de clientes de diversas formas.

A redução do tempo de triagem permite que tickets sejam classificados em segundos, liberando a equipe para a resolução efetiva.

A priorização baseada em critérios objetivos faz com que clientes em situação crítica ou com risco de churn sejam atendidos primeiro, reduzindo cancelamentos evitáveis.

A padronização do atendimento reduz a variabilidade entre atendentes e melhora a consistência da experiência, pois o modelo sugere tom e ação recomendada.

A geração de dados estruturados alimenta indicadores de saúde da base, como categorias mais frequentes, sentimento médio e volume de tickets críticos, apoiando decisões de produto e de customer success.

A escalabilidade do suporte permite que a operação cresça sem necessidade de aumento proporcional do time de triagem.

## 7. Riscos

A automação dessa tarefa envolve riscos que devem ser medidos e mitigados.

A classificação incorreta encaminha o ticket para a equipe errada, atrasando a resolução.

A urgência subestimada faz com que um problema crítico seja tratado como baixa prioridade, podendo gerar indisponibilidade prolongada ou perda do cliente.

O falso risco de churn sinaliza clientes incorretamente, desperdiçando esforço do time de customer success, ou deixa clientes em risco sem ação, no sentido oposto.

A alucinação ocorre quando o modelo inventa fatos não presentes no ticket, como citar funcionalidades, valores ou prazos inexistentes.

O encaminhamento incorreto, derivado de classificação errada, gera retrabalho e frustração do cliente.

A saída fora do formato quebra integrações automatizadas e exige tratamento de exceção, quando a resposta não é JSON válido.

A dependência excessiva de automação pode gerar perdas financeiras e reputacionais quando decisões sensíveis, como cancelamento ou segurança, são tomadas sem supervisão humana.

## 8. Modelos avaliados

Os três modelos candidatos à seleção são DeepSeek V4 Flash, Kimi K2.7 e Kimi K3.

Nesta etapa não são apresentadas características técnicas ou comerciais desses modelos, como arquitetura, tamanho de contexto, preços ou limites de taxa. Tais informações ainda não foram verificadas. Todos os dados relevantes serão coletados durante a execução do benchmark e registrados na tabela comparativa de métricas, no arquivo 03_tabela_comparativa_metricas.csv.
