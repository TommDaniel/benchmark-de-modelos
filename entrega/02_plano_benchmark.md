# Plano de Benchmark

Comparação de modelos de IA para análise automatizada de tickets e feedbacks em produto SaaS B2B.

## 1. Objetivo

O objetivo é comparar os modelos DeepSeek V4 Flash, Kimi K2.7 e Kimi K3 nos seguintes critérios: qualidade da análise, custo por requisição e em escala, velocidade medida pela latência, consistência entre execuções, adequação ao formato de saída e ao tom, e risco de erro para uso empresarial.

Os testes serão executados na plataforma OpenCode, com os mesmos dados e o mesmo prompt, garantindo condições comparáveis.

## 2. Hipótese

A hipótese de trabalho é a seguinte: modelos com maior custo podem apresentar melhor qualidade, mas o ganho pode não justificar o impacto econômico no produto SaaS.

A hipótese é neutra e será confirmada ou refutada exclusivamente pelos dados coletados.

## 3. Dataset

Será utilizado um conjunto de 5 tickets de clientes, com a seguinte distribuição recomendada: 3 casos comuns, 1 caso ambíguo e 1 caso difícil ou crítico.

Os casos comuns são situações rotineiras de suporte, com classificação relativamente direta.

O caso ambíguo é um texto com múltiplas interpretações possíveis, como uma reclamação que mistura cobrança e problema técnico.

O caso difícil ou crítico é uma situação sensível, como ameaça explícita de cancelamento, incidente de segurança ou indisponibilidade grave.

Os casos devem contemplar as seguintes categorias: problemas técnicos, cobrança, integrações, usabilidade, pedidos de funcionalidades, intenção de cancelamento, segurança, reclamações e dúvidas comerciais.

Caso sejam utilizados dados reais de clientes, todos os dados pessoais e sensíveis, como nomes, e-mails, empresas, identificadores e valores contratuais, devem ser removidos ou substituídos por dados fictícios antes dos testes.

Para cada ticket, deve ser definido previamente um gabarito de referência, contendo categoria, sentimento, urgência e risco de churn esperados. O gabarito deve ser revisado por pelo menos uma pessoa para permitir o cálculo de acurácia.

## 4. Condições equivalentes

Para que a comparação seja justa, todos os modelos deverão utilizar os mesmos tickets, o mesmo prompt e o mesmo formato de saída, que é o JSON definido na descrição da tarefa.

Os modelos deverão utilizar a mesma ordem de testes, ou uma ordem alternada e documentada. Deverão utilizar o mesmo limite máximo de resposta, quando possível, e temperatura zero ou o menor valor disponível.

Cada ticket será executado três vezes para cada modelo, com o objetivo de medir consistência. As condições de rede devem ser semelhantes, e a medição da latência deve seguir o mesmo ponto de início e fim da contagem.

Quando algum parâmetro não puder ser controlado ou igualado no OpenCode, como parâmetro de temperatura indisponível, limite de tokens diferente ou forma de medição de latência da plataforma, isso deverá ser registrado explicitamente como limitação no relatório final.

## 5. Prompt padrão do benchmark

O prompt abaixo deve ser copiado e utilizado, sem alterações, nos três modelos. Substitua apenas o campo [TICKET_DO_CLIENTE] pelo texto do ticket em avaliação.

```text
Você é um analista de suporte de um produto SaaS B2B.

Analise EXCLUSIVAMENTE o conteúdo do ticket do cliente abaixo e produza uma
análise estruturada.

Regras obrigatórias:
1. Baseie-se apenas nas informações presentes no ticket. NÃO invente fatos,
   valores, datas, funcionalidades ou contextos que não estejam no texto.
2. Responda SOMENTE com um JSON válido. Não inclua nenhum texto antes ou
   depois do JSON (sem explicações, sem markdown, sem blocos de código).
3. Utilize somente os valores permitidos em cada campo, conforme a lista abaixo.
4. Mantenha a justificativa curta (no máximo 2 frases) e baseada no texto.
5. Avalie com atenção a urgência e o risco de churn: sinais de frustração
   intensa, ameaça de cancelamento, indisponibilidade ou impacto financeiro
   devem elevar esses campos.

Estrutura obrigatória da resposta:
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

TICKET DO CLIENTE:
[TICKET_DO_CLIENTE]
```

## 6. Repetições

Nesta atividade, cada ticket será executado três vezes por modelo. Com 5 tickets, isso representa 45 execuções, calculadas como 5 tickets multiplicados por 3 modelos e por 3 repetições.

O benchmark piloto foi executado com 5 tickets em três rodadas por modelo. A consistência entre repetições foi medida e registrada na tabela comparativa. Para decisão de produção, recomenda-se ampliar o dataset para 30 tickets mantendo as três repetições.

## 7. Métricas

### 7.1 Custo estimado por requisição

A fórmula do custo é a seguinte:

```text
Custo =
(tokens de entrada ÷ 1.000.000 × preço de entrada)
+
(tokens de saída ÷ 1.000.000 × preço de saída)
```

O preço deve ser preenchido conforme o valor efetivamente cobrado pela plataforma utilizada no momento da execução. Não devem ser utilizados preços de fontes não verificadas.

A partir do custo médio por requisição, deve ser projetado o custo mensal para 10 mil requisições, 100 mil requisições e 1 milhão de requisições.

### 7.2 Latência

A latência é o tempo entre o envio da solicitação e o recebimento completo da resposta, medido de forma equivalente nas duas plataformas. Além da média, recomenda-se registrar a mediana, o percentil 95, o valor mínimo, o valor máximo, a quantidade e a taxa de timeout, e a quantidade e a taxa de erro técnico.

### 7.3 Qualidade da saída

A qualidade da saída deve ser avaliada em comparação com o gabarito de referência, observando a correção da categoria, a correção do sentimento, a correção da urgência, a correção do risco de churn, a fidelidade do resumo sem omissões relevantes nem invenções, a qualidade da ação recomendada e a ausência de informações inventadas.

### 7.4 Consistência

A consistência verifica se o mesmo modelo apresenta respostas semelhantes nas três execuções do mesmo ticket. Deve ser registrado o percentual de tickets em que as execuções produziram a mesma classificação nos campos categoria, sentimento, urgência e risco de churn.

### 7.5 Adequação ao tom e formato

A adequação ao tom e formato deve avaliar a validade do JSON, ou seja, se ele é parseável, a presença de todos os campos obrigatórios, o uso exclusivo das categorias e valores permitidos, a objetividade, a clareza, a adequação do tom sugerido e a ausência de texto fora do formato.

### 7.6 Risco percebido de erro

O risco percebido de erro utiliza uma escala de 1 a 5, conforme a tabela a seguir.

| Nota | Interpretação |
|------|----------------|
| 1 | Risco muito baixo |
| 2 | Risco baixo |
| 3 | Risco moderado |
| 4 | Risco alto |
| 5 | Risco crítico |

## 8. Avaliação humana

A rubrica de avaliação humana utiliza escala de 1 a 5, em que 1 significa muito ruim e 5 significa excelente. Ela é aplicada a uma amostra das respostas.

| Critério | 1 | 3 | 5 |
|----------|---|---|---|
| Fidelidade do resumo | Resumo incorreto ou inventado | Resumo parcialmente fiel | Resumo fiel e completo |
| Correção da classificação | Classificação incorreta | Parcialmente correta | Totalmente correta |
| Qualidade da ação recomendada | Ação inadequada ou ausente | Ação genérica | Ação específica e adequada |
| Adequação do tom | Tom inadequado à situação | Tom aceitável | Tom ideal para o caso |
| Risco percebido | Erro crítico provável | Risco moderado | Uso seguro |
| Segurança para uso no produto | Inseguro sem revisão | Usável com supervisão | Usável com alta confiança |

Recomenda-se que os nomes dos modelos sejam ocultados durante a avaliação humana sempre que possível, identificando as respostas apenas por códigos, como Modelo A, Modelo B e Modelo C, para evitar viés.

## 9. Critérios eliminatórios sugeridos

Os valores iniciais podem ser ajustados conforme a estratégia da empresa. Os critérios sugeridos são: pelo menos 98% de respostas em JSON válido, no máximo 5% de erros críticos, pelo menos 80% de acurácia nas classificações, e percentil 95 de latência dentro do limite aceitável para o produto. O limite de latência deve ser definido pela equipe antes da execução.

Um modelo que não atingir qualquer um desses critérios deve ser marcado como reprovado nos critérios eliminatórios, independentemente da pontuação ponderada.

## 10. Pesos da decisão

A tabela a seguir apresenta os pesos sugeridos para cada critério.

| Critério | Peso |
|----------|------|
| Qualidade | 30% |
| Risco de erro | 20% |
| Custo | 20% |
| Latência | 15% |
| Consistência | 10% |
| Adequação ao formato e tom | 5% |

A pontuação final, de 0 a 100, será calculada pela soma ponderada das notas normalizadas de cada critério. Os pesos podem ser alterados conforme a estratégia da empresa, como aumentar o peso de custo em cenários de alto volume ou o peso de risco em cenários regulados.

## 11. Procedimento de execução

O procedimento de execução é o seguinte:

Selecionar os tickets conforme a distribuição definida, com 3 comuns, 1 ambíguo e 1 difícil ou crítico, anonimizando dados reais.

Revisar os resultados esperados, definindo o gabarito de referência de cada ticket.

Executar o mesmo prompt no OpenCode, para cada modelo, garantindo as condições equivalentes da seção 4.

Executar cada ticket três vezes por modelo, registrando cada execução individualmente.

Registrar os resultados de cada execução, incluindo resposta bruta, tokens de entrada e saída, latência, erros técnicos e timeouts.

Preencher a tabela comparativa, no arquivo 03_tabela_comparativa_metricas.csv, com as métricas consolidadas.

Calcular médias e demais agregações, como mediana, percentil 95 e taxas percentuais.

Analisar os trade-offs entre qualidade, custo, latência, consistência e risco.

Gerar a recomendação final no relatório 04_relatorio_final.pdf, aplicando os critérios eliminatórios e a pontuação ponderada.
