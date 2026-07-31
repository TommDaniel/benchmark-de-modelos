# Plano de Benchmark

**Comparação de modelos de IA para análise automatizada de tickets e feedbacks — SaaS B2B**

---

## 1. Objetivo

Comparar os modelos **DeepSeek V4 Flash**, **Kimi K2.7** e **Kimi K3** nos seguintes critérios:

- qualidade da análise;
- custo por requisição e em escala;
- velocidade (latência);
- consistência entre execuções;
- adequação ao formato de saída e ao tom;
- risco de erro para uso empresarial.

Os testes serão executados na plataforma **OpenCode**, com os mesmos dados e o mesmo prompt, garantindo condições comparáveis.

## 2. Hipótese

> Modelos com maior custo podem apresentar melhor qualidade, mas o ganho pode não justificar o impacto econômico no produto SaaS.

A hipótese é neutra e será confirmada ou refutada exclusivamente pelos dados coletados.

## 3. Dataset

Será utilizado um conjunto de **5 tickets** de clientes, com a seguinte distribuição recomendada:

- **3 casos comuns:** situações rotineiras de suporte, com classificação relativamente direta;
- **1 caso ambíguo:** texto com múltiplas interpretações possíveis (ex.: reclamação que mistura cobrança e problema técnico);
- **1 caso difícil ou crítico:** situação sensível, como ameaça explícita de cancelamento, incidente de segurança ou indisponibilidade grave.

Os casos devem contemplar as seguintes categorias:

- problemas técnicos;
- cobrança;
- integrações;
- usabilidade;
- pedidos de funcionalidades;
- intenção de cancelamento;
- segurança;
- reclamações;
- dúvidas comerciais.

**Anonimização:** caso sejam utilizados dados reais de clientes, todos os dados pessoais e sensíveis (nomes, e-mails, empresas, identificadores, valores contratuais) devem ser removidos ou substituídos por dados fictícios antes dos testes.

Para cada ticket, deve ser definido previamente um **gabarito de referência** (categoria, sentimento, urgência e risco de churn esperados), revisado por pelo menos uma pessoa, para permitir o cálculo de acurácia.

## 4. Condições equivalentes

Para que a comparação seja justa, todos os modelos deverão utilizar:

- os mesmos tickets;
- o mesmo prompt;
- o mesmo formato de saída (JSON definido na descrição da tarefa);
- a mesma ordem de testes, ou ordem alternada e documentada;
- o mesmo limite máximo de resposta, quando possível;
- temperatura zero ou o menor valor disponível;
- três execuções para cada ticket (para medir consistência);
- condições de rede semelhantes;
- medição equivalente da latência (mesmo ponto de início e fim da contagem).

Quando algum parâmetro não puder ser controlado ou igualado no OpenCode (ex.: parâmetro de temperatura indisponível, limite de tokens diferente, forma de medição de latência da plataforma), isso deverá ser **registrado explicitamente como limitação** no relatório final.

## 5. Prompt padrão do benchmark

O prompt abaixo deve ser copiado e utilizado, sem alterações, nos três modelos. Substitua apenas o campo `[TICKET_DO_CLIENTE]` pelo texto do ticket em avaliação.

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

Nesta atividade, cada ticket será executado **três vezes por modelo**. Com 5 tickets, isso representa:

```text
5 tickets × 3 modelos × 3 repetições = 45 execuções
```

> **Nota:** o benchmark piloto foi executado com 5 tickets em três rodadas por modelo. A consistência entre repetições foi medida e registrada na tabela comparativa. Para decisão de produção, recomenda-se ampliar o dataset para 30 tickets mantendo as três repetições.

## 7. Métricas

### 7.1 Custo estimado por requisição

Fórmula:

```text
Custo =
(tokens de entrada ÷ 1.000.000 × preço de entrada)
+
(tokens de saída ÷ 1.000.000 × preço de saída)
```

O preço deve ser preenchido conforme o valor **efetivamente cobrado pela plataforma utilizada** no momento da execução. Não utilizar preços de fontes não verificadas.

A partir do custo médio por requisição, projetar o custo mensal para:

- 10 mil requisições;
- 100 mil requisições;
- 1 milhão de requisições.

### 7.2 Latência

Tempo entre o envio da solicitação e o recebimento completo da resposta, medido de forma equivalente nas duas plataformas. Além da **média**, recomenda-se registrar:

- mediana;
- p95;
- valor mínimo;
- valor máximo;
- timeout (quantidade e taxa);
- erro técnico (quantidade e taxa).

### 7.3 Qualidade da saída

Avaliar, em comparação com o gabarito de referência:

- correção da categoria;
- correção do sentimento;
- correção da urgência;
- correção do risco de churn;
- fidelidade do resumo (sem omissões relevantes nem invenções);
- qualidade da ação recomendada;
- ausência de informações inventadas.

### 7.4 Consistência

Verificar se o mesmo modelo apresenta respostas semelhantes nas três execuções do mesmo ticket. Registrar o percentual de tickets em que as execuções produziram a mesma classificação nos campos categoria, sentimento, urgência e risco de churn.

### 7.5 Adequação ao tom e formato

Avaliar:

- validade do JSON (parseável);
- presença de todos os campos obrigatórios;
- uso exclusivo das categorias e valores permitidos;
- objetividade;
- clareza;
- adequação do tom sugerido;
- ausência de texto fora do formato.

### 7.6 Risco percebido de erro

Escala de 1 a 5:

| Nota | Interpretação |
|------|----------------|
| 1 | Risco muito baixo |
| 2 | Risco baixo |
| 3 | Risco moderado |
| 4 | Risco alto |
| 5 | Risco crítico |

## 8. Avaliação humana

Rubrica de avaliação de 1 a 5 (1 = muito ruim, 5 = excelente) aplicada a uma amostra das respostas:

| Critério | 1 | 3 | 5 |
|----------|---|---|---|
| Fidelidade do resumo | Resumo incorreto ou inventado | Resumo parcialmente fiel | Resumo fiel e completo |
| Correção da classificação | Classificação incorreta | Parcialmente correta | Totalmente correta |
| Qualidade da ação recomendada | Ação inadequada ou ausente | Ação genérica | Ação específica e adequada |
| Adequação do tom | Tom inadequado à situação | Tom aceitável | Tom ideal para o caso |
| Risco percebido | Erro crítico provável | Risco moderado | Uso seguro |
| Segurança para uso no produto | Inseguro sem revisão | Usável com supervisão | Usável com alta confiança |

**Recomendação:** os nomes dos modelos devem ser **ocultados durante a avaliação humana** sempre que possível (avaliação cega), identificando as respostas apenas por códigos (ex.: Modelo A, Modelo B, Modelo C), para evitar viés.

## 9. Critérios eliminatórios sugeridos

Valores iniciais — **podem ser ajustados** conforme a estratégia da empresa:

- pelo menos **98%** de respostas em JSON válido;
- no máximo **5%** de erros críticos;
- pelo menos **80%** de acurácia nas classificações;
- **p95 de latência** dentro do limite aceitável para o produto (limite a ser definido pela equipe antes da execução).

Um modelo que não atingir qualquer um desses critérios deve ser marcado como reprovado nos critérios eliminatórios, independentemente da pontuação ponderada.

## 10. Pesos da decisão

| Critério | Peso |
|----------|------|
| Qualidade | 30% |
| Risco de erro | 20% |
| Custo | 20% |
| Latência | 15% |
| Consistência | 10% |
| Adequação ao formato e tom | 5% |

A pontuação final (0 a 100) será calculada pela soma ponderada das notas normalizadas de cada critério. **Os pesos podem ser alterados** conforme a estratégia da empresa (ex.: aumentar o peso de custo em cenários de alto volume, ou o peso de risco em cenários regulados).

## 11. Procedimento de execução

1. **Selecionar os tickets** conforme a distribuição definida (3 comuns, 1 ambíguo, 1 difícil/crítico), anonimizando dados reais.
2. **Revisar os resultados esperados**, definindo o gabarito de referência de cada ticket.
3. **Executar o mesmo prompt** no OpenCode, para cada modelo, garantindo as condições equivalentes da seção 4.
4. **Executar cada ticket três vezes por modelo**, registrando cada execução individualmente.
5. **Registrar os resultados**: resposta bruta, tokens de entrada e saída, latência, erros técnicos e timeouts.
6. **Preencher a tabela comparativa** (`03_tabela_comparativa_metricas.csv`) com as métricas consolidadas.
7. **Calcular médias** e demais agregações (mediana, p95, taxas percentuais).
8. **Analisar os trade-offs** entre qualidade, custo, latência, consistência e risco.
9. **Gerar a recomendação final** no relatório (`04_relatorio_final.pdf`), aplicando os critérios eliminatórios e a pontuação ponderada.
