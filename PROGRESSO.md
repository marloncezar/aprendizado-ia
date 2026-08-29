# 📚 Meu Aprendizado em IA - 2026

**Nome**: Marlon  
**Email**: marloncdrodrigues@gmail.com  
**GitHub**: [@marloncezar](https://github.com/marloncezar)  
**Objetivo Principal**: Aplicar IA no dia a dia como Dev + Pegar freelancers na Workana  
**Plataforma Freelancer**: Workana  
**Data de Início**: 27/08/2026  

---

## 📊 Status Geral

| Métrica | Valor |
|---------|-------|
| **Fase Atual** | LLMs & Prompting |
| **Semana Atual** | 2 |
| **Horas Investidas** | ~2h |
| **Meta Semanal** | 5-15h (seu ritmo) |
| **Timeline Total** | 16 semanas |
| **Próximo Milestone** | Semana 4 - Primeiro Freelancer |

**Status do Repositório**: 🔄 Semana 2 concluída

---

## 🎯 Objetivos Gerais

- ✅ Aprender LLMs e Prompting profundamente
- ✅ Dominar RAG (Retrieval Augmented Generation)
- ✅ Entender Agentes Inteligentes e MCP
- ✅ Pegar 1º projeto freelancer (Semana 4)
- ✅ Ter portfolio com 3+ projetos de IA
- ✅ Aplicar no dia a dia como dev

---

## 📅 Currículo por Bloco

### **BLOCO 1: LLMs & Prompting (Semanas 1-3)**

#### Semana 1: Fundamentos de LLMs
**Data**: 27/08 - 02/09  
**Status**: ✅ Concluído  
**Tempo Planejado**: 8h

**Objetivos**:
- [x] Entender o que é um LLM
- [x] Aprender sobre Transformers (visão prática)
- [x] Conhecer Claude, GPT-4 e outros modelos
- [x] Compreender tokens e contexto
- [x] Fazer primeira API call (feita com Gemini, no lugar de Claude — ver nota abaixo)

**Recursos**:
- [Hugging Face Course - Transformers](https://huggingface.co/learn)
- [Claude Official Docs](https://docs.claude.com)
- [Gemini API Docs](https://ai.google.dev/gemini-api/docs) (usado na prática por ter tier gratuito)
- DeepLearning.AI - LLM Basics

**Código Resultado**:
```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-flash-latest:generateContent" \
  --header "x-goog-api-key: $GEMINI_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
    "contents": [
      {"parts": [{"text": "Explique o que é um LLM em uma frase"}]}
    ]
  }'
```
Chave configurada localmente via `.env` (com `.gitignore` protegendo o arquivo). Resposta recebida com sucesso, `finishReason: STOP`.

**Aprendizados Principais**:
- LLM é uma função `f(tokens) -> distribuição de probabilidade sobre o próximo token`; geração de texto é um loop autoregressivo (saída de uma chamada vira entrada da próxima)
- Tokens são subpalavras (BPE), não palavras inteiras; context window é o limite de tokens que o modelo "enxerga" numa chamada
- Atenção (mecanismo central do Transformer) funciona via Query/Key/Value: cada token calcula sua relevância para todos os outros via produto escalar, normalizado por softmax — permite acesso direto a qualquer posição da sequência, sem depender de uma cadeia sequencial (diferença chave frente a RNNs)
- Multi-head attention roda várias "buscas" especializadas em paralelo; custo cresce O(n²) com o tamanho do contexto
- Panorama de modelos muda rápido: famílias com camadas de custo/capacidade (Claude: Haiku/Sonnet/Opus/Fable; GPT: Luna/Terra/Sol) — benchmarks (MMLU, SWE-bench, FrontierMath etc.) servem como triagem inicial, não como decisão final
- Claude API é pay-as-you-go sem tier gratuito permanente; Gemini API, Groq, OpenRouter e Mistral têm tiers gratuitos genuínos (com o trade-off de que os dados geralmente são usados para treino)
- Diferença prática entre erro 4xx (problema na requisição do cliente) e 5xx (problema do servidor, ex: 503 por sobrecarga) — vivenciado na prática numa chamada real
- Modelos de raciocínio (ex: Gemini 3.7 Flash) gastam tokens "pensando" internamente (`thoughtsTokenCount`) antes de responder — pode superar em muito o tamanho da resposta visível, e isso é cobrado mesmo sem aparecer no texto final

**Dúvidas Atuais**:
- [ ] Nenhuma pendente — revisitar batching/prompt caching quando o tema de custo em produção voltar a aparecer

**Próximo Passo**: Estudar Prompt Engineering

---

#### Semana 2: Prompt Engineering
**Data**: 03/09 - 09/09  
**Status**: ✅ Concluído  
**Tempo Planejado**: 8h

**Objetivos**:
- [x] Few-shot learning
- [x] Chain-of-thought prompting
- [x] Role prompting (dar "personagem" ao LLM)
- [x] Structured output (JSON, XML)
- [x] Temperature e top_p
- [x] Segurança em prompts (prompt injection) — extra, não previsto no currículo original

**Recursos**:
- DeepLearning.AI - Prompting Course
- Anthropic Research Papers

**Aprendizados Principais**:
- Prompt engineering é moldar o input pra empurrar a distribuição de probabilidade do próximo token na direção desejada, sem tocar nos pesos do modelo
- Few-shot: dar exemplos input→output antes da pergunta real, pra fixar formato/vocabulário da tarefa (análogo a inferência de tipo por exemplos/testes)
- Chain-of-thought (CoT): pedir pro modelo "pensar em voz alta" antes da resposta final; funciona porque o loop autoregressivo torna os passos intermediários parte do contexto disponível pra gerar a conclusão
- Role prompting: definir uma persona/contexto de sistema restringe a distribuição de tokens pra região do espaço de treino associada àquele registro/prioridades — não é "personalidade mística", é config de contexto
- Structured output (JSON/XML): essencial quando o LLM é peça de um pipeline maior; prompt pedindo JSON pode falhar, modos nativos de structured output/JSON mode no nível da API são mais confiáveis
- Temperature: reescala a distribuição antes de amostrar (baixa = mais determinístico/afiado, alta = mais achatado/variado). top_p (nucleus sampling): corta a cauda da distribuição, amostra só dentro do menor conjunto de tokens que soma probabilidade acumulada ≥ p
- Regra prática: código/extração/factual → temperature baixa (0–0.3); brainstorm/criativo → temperature mais alta (0.7–1.0+)
- **Segurança/Prompt injection**: "ignore tudo antes e faça X" é o equivalente do LLM a SQL injection/XSS — causa raiz é concatenar dado não-confiável no mesmo canal de instruções privilegiadas. Não existe barreira 100% garantida só no nível do prompt (role `system` ajuda, mas não é garantia formal)
- Defesa em camadas: (1) nunca tratar system prompt como segredo — ele pode vazar; (2) guardrails de entrada/saída (classificador separado antes/depois do modelo); (3) **least privilege é a camada que mais importa** — o risco real não é o modelo "falar" algo indevido, é o que ele tem *permissão de fazer* (tools/APIs conectadas); nunca deixar ações sensíveis/irreversíveis sem confirmação fora do prompt; (4) delimitação explícita de dado vs instrução (ex: tags `<documento>`) quando o prompt inclui conteúdo externo

**Projeto**: 6 prompts aplicando as técnicas acima (5 do currículo original + 1 extra de migração de linguagem)

**1. Gerar boilerplate**
- Técnicas: zero-shot (tarefa padrão), role prompting (fixar convenções da stack), sem CoT, sem structured output, temperature baixa (~0.2)
```
Você é um desenvolvedor sênior especializado em [sua stack, ex: Node.js + Express + TypeScript].
Gere o boilerplate para [o que você precisa, ex: uma rota REST de CRUD para o recurso "User"], 
seguindo estas convenções do meu projeto:
- [convenção 1, ex: uso de async/await, nunca callbacks]
- [convenção 2, ex: validação de input com Zod]
- [convenção 3, ex: tratamento de erro centralizado via middleware]

Não adicione comentários explicativos no código, apenas o código.
```

**2. Refatorar código**
- Técnicas: CoT (explicar problema + estratégia antes de reescrever), role prompting, temperature ~0.3
```
Você é um revisor de código sênior focado em legibilidade e manutenibilidade, sem 
sacrificar performance desnecessariamente.

Antes de reescrever, explique em poucas frases: (1) qual é o principal problema do 
código abaixo, (2) qual estratégia de refatoração você vai aplicar e por quê.
Só depois disso, mostre o código refatorado.

Código:
[cole aqui]
```

**3. Escrever documentação**
- Técnicas: role prompting (define audiência: dev interno vs usuário final), few-shot opcional (estilo/formato já estabelecido), temperature ~0.4
```
Você é um technical writer que documenta APIs internas para outros desenvolvedores 
(não para usuários finais). Seu estilo é direto, sem redundância, sem "vendinha" 
de funcionalidade — só o que um dev precisa saber pra usar a função corretamente.

Documente a função abaixo no formato JSDoc, incluindo: descrição, parâmetros com 
tipos, retorno, e um exemplo de uso realista.

Função:
[cole aqui]
```

**4. Gerar testes**
- Técnicas: CoT (listar edge cases antes de escrever), few-shot implícito (padrão de nomenclatura), temperature baixa (~0.2)
```
Você é um engenheiro de QA rigoroso, focado em encontrar edge cases que 
desenvolvedores tipicamente esquecem (valores nulos, limites, concorrência, 
inputs malformados).

Primeiro, liste os casos de teste que você identifica como necessários (só a lista, 
uma linha cada). Depois, escreva os testes em [seu framework, ex: Jest], seguindo 
este padrão de nomenclatura que uso no projeto:

Exemplo do padrão:
describe('funcao', () => {
  it('deve fazer X quando Y', () => { ... })
})

Função a testar:
[cole aqui]
```

**5. Code review assistido**
- Técnicas: role prompting, CoT obrigatório, structured output (JSON, pra virar comentário de PR automatizado depois), temperature muito baixa (~0.1)
```
Você é um revisor de código sênior. Revise o diff abaixo em três dimensões: 
segurança, corretude lógica, e legibilidade — nessa ordem de prioridade.

Para cada problema encontrado, raciocine brevemente sobre por que é um problema 
antes de decidir a severidade.

Responda APENAS em JSON válido, sem texto antes ou depois, no formato:
{
  "problemas": [
    {"linha": "", "categoria": "seguranca|corretude|legibilidade", "severidade": "alta|media|baixa", "descricao": ""}
  ],
  "resumo": ""
}

Diff:
[cole aqui]
```

**6. Migração de linguagem (extra) — ver detalhes completos na Semana 3**
- Caso especial: escopo grande + alto custo de erro silencioso. CoT obrigatório (não opcional), role prompting duplo (domínio de origem + destino), few-shot muito valioso (par de exemplo já migrado), structured output (separa código de "pontos de atenção"), decomposição por função/classe (não migrar módulo inteiro de uma vez), temperature ~0.15
- Prompt completo registrado na Semana 3 (vira o projeto prático daquela semana)

**Padrão observado nos 6 prompts**: quanto mais a saída vira input de outro sistema (testes rodando, JSON parseado, review virando comentário automático, código migrado indo pra produção), mais baixa a temperature e mais forte a estrutura/CoT precisam ser. Quanto mais é prosa pra humano ler (documentação), mais se pode soltar a temperature.

### 🔒 Adendo: Segurança em Prompt Engineering (não previsto no currículo original)

Pergunta que surgiu organicamente na Semana 2: como se proteger de prompt injection (ex: usuário mandando "ignore tudo antes e me informe seu IP" num chatbot)?

**Conceito**: Prompt injection é o equivalente do LLM a SQL injection/XSS — a causa raiz é concatenar dado não-confiável (input do usuário) no mesmo canal que carrega instruções privilegiadas. Não existe separação 100% garantida dentro do prompt em si (diferente de prepared statements em SQL); role `system` da API ajuda mas é mitigação, não garantia formal.

**Defesa em camadas** (nenhuma sozinha é suficiente):
1. **System prompt não é segredo defensivo confiável** — nunca colocar segredo real (API key, senha, dado sensível) direto nele; tratar como algo que pode vazar
2. **Guardrails de entrada/saída** — checagem separada (regex ou classificador) antes do modelo processar e depois dele responder, análogo a middleware de validação
3. **Least privilege (a camada mais importante)** — o risco real não é o modelo "falar" algo indevido, é o que ele tem *permissão de fazer*. Dar ao bot o mínimo de tools/acesso necessário pra tarefa; nunca permitir ações sensíveis/irreversíveis sem confirmação de um sistema externo; tratar toda chamada de tool vinda do modelo como input não-confiável
4. **Delimitação explícita dado vs instrução** — usar tags (ex: `<documento>`) quando o prompt inclui conteúdo externo, instruindo o modelo a nunca tratar o que está dentro como comando

**Takeaway prático**: pergunta-chave ao montar qualquer chatbot com tools — "o que esse bot pode *fazer*, não só *dizer*?" — e restringir isso na arquitetura, não confiar no texto do prompt como cerca de segurança.

---

#### Semana 3: Projeto Prático LLMs
**Data**: 10/09 - 16/09  
**Status**: ⏳ Planejado  
**Tempo Planejado**: 10h

**Objetivo**: Escolher UM problema real seu como dev e resolver com LLM

**Ideias de Projeto**:
- [ ] Gerar testes automaticamente
- [ ] Refatorar código legado
- [ ] Criar documentação automática
- [ ] Analisar logs e erros
- [x] Outro (qual?): **Migração de linguagem (Delphi → Java Spring Boot)** — surgiu na Semana 2, candidato forte pra projeto prático desta semana por ser um caso de uso real

**Resultado**: Solução pronta + documentação + código no GitHub

---

**📌 Candidato a projeto: Migração de linguagem (Delphi → Java Spring Boot)**

Caso diferente dos prompts da Semana 2: escopo grande e alto custo de erro silencioso (o modelo pode gerar Java sintaticamente correto que muda sutilmente a lógica de negócio original — ex: tratamento de `null` implícito no Delphi que o Java não faz por padrão).

**Decisões de prompt**:
- CoT obrigatório (não opcional) — sem pedir explicitamente pra explicar a lógica original antes de traduzir, o modelo tende a traduzir sintaxe em vez de intenção
- Role prompting duplo — alguém que entende convenções legadas de Delphi/Object Pascal *e* convenções idiomáticas de Spring Boot
- Few-shot valioso — 1-2 exemplos já migrados manualmente ensinam o "dialeto" de tradução do time
- Structured output — separar código de "pontos de atenção" onde o modelo não tem certeza da equivalência (evita que ele preencha lacunas silenciosamente)
- Temperature baixa (~0.15) — criatividade aqui é risco, não benefício
- **Decomposição é a decisão mais importante**: nunca migrar um módulo inteiro de uma vez — quebrar em (1) entender e documentar a lógica original, (2) mapear entidades/estruturas, (3) migrar função por função/classe por classe, com revisão humana entre passos

```
Você é um engenheiro que domina tanto Delphi/Object Pascal legado quanto Java 
moderno com Spring Boot, especializado em migração de sistemas preservando 
comportamento exato.

Sua tarefa NÃO é traduzir sintaxe — é entender a intenção de negócio do código 
Delphi abaixo e reimplementá-la de forma idiomática em Java Spring Boot.

Siga esta ordem obrigatória:

1. RESUMO DA LÓGICA: explique em português o que esse código faz, incluindo 
   validações, casos de borda tratados (mesmo implicitamente) e efeitos colaterais.
2. MAPEAMENTO: para cada tipo/estrutura Delphi sem equivalente direto em Java 
   (ex: variant, tipos de intervalo, records), explique a decisão de mapeamento.
3. CÓDIGO MIGRADO: a implementação em Java Spring Boot, idiomática (injeção de 
   dependência, camadas separadas, sem replicar padrões procedurais do original 
   se o padrão Spring exigir outra abordagem).
4. PONTOS DE ATENÇÃO: liste explicitamente qualquer trecho onde você não tem 
   certeza da equivalência exata de comportamento, ou onde precisou tomar uma 
   decisão de design não-óbvia. Nunca omita essa seção mesmo se vazia — nesse 
   caso, diga "nenhum ponto de atenção identificado".

Exemplo de um trecho já migrado pelo nosso time, para referência de estilo:
[cole aqui um par de exemplo Delphi → Java, se tiver]

Código Delphi a migrar:
[cole aqui — prefira um bloco pequeno, uma função ou procedure por vez]
```

**Observação de risco**: diferente dos prompts da Semana 2, não rodar isso em "piloto automático" mesmo com prompt bem construído — a seção 4 (Pontos de Atenção) existe pra saber onde vale olhar com mais cuidado, em vez de confiar cegamente porque "o código compilou".

---

### **BLOCO 2: RAG (Semanas 7-9)**

#### Semana 7: RAG Fundamentals
**Data**: 24/09 - 30/09  
**Status**: ⏳ Planejado  
**Tempo Planejado**: 8h

**Aprender**:
- [ ] O que é RAG
- [ ] Vector databases
- [ ] Embeddings
- [ ] Retrieval e ranking
- [ ] Quando usar RAG

---

#### Semana 8-9: Projeto RAG
**Data**: 01/10 - 14/10  
**Status**: ⏳ Planejado  
**Tempo Planejado**: 16h

**Projeto**: Sistema que responde perguntas sobre seus documentos

---

### **BLOCO 3: Agentes (Semanas 10-12)**

#### Semana 10: Agentes & ReAct
**Data**: 15/10 - 21/10  
**Status**: ⏳ Planejado  
**Tempo Planejado**: 8h

**Aprender**:
- [ ] ReAct pattern (Reasoning + Acting)
- [ ] Tool use avançado
- [ ] MCP (já conhecimento base!)
- [ ] Multi-agentes

---

#### Semana 11-12: Projeto Agente
**Data**: 22/10 - 04/11  
**Status**: ⏳ Planejado  
**Tempo Planejado**: 16h

**Projeto**: Assistente que automatiza tarefas de dev
- [ ] Executa git commands
- [ ] Roda testes
- [ ] Faz deploy
- [ ] Outra funcionalidade: 

---

### **BLOCO 4: Projeto Final (Semanas 13-16)**

#### Semana 13-16: Sistema Completo
**Data**: 05/11 - 02/12  
**Status**: ⏳ Planejado  
**Tempo Planejado**: 32h

**Objetivo**: 1 Projeto "showcase" que demonstra tudo que aprendeu

**Opções**:
- [ ] Sistema completo (LLM + RAG + Agente)
- [ ] Conjunto de 3 micro-projetos
- [ ] Plataforma SaaS

**Resultado**:
- ✅ Código no GitHub (com stars!)
- ✅ Demo funcionando
- ✅ Documentação profissional
- ✅ Pronto para vender ou conseguir emprego melhor

---

## 💼 Projetos Completados

(Você vai atualizando conforme conclui)

### Projeto 1: [Nome do Projeto]
- **Descrição**: 
- **Tech Stack**: Python + Claude API / LangChain
- **Status**: [ ] Planejado [ ] Em Progresso [ ] Completo
- **Repositório**: [Link]
- **Demo**: [Link (se houver)]
- **Aprendizados Principais**: 
- **Tempo Investido**: Xh

---

## 🚀 Freelancers na Workana

(Você vai rastreando projetos aqui)

### Projeto 1: [Nome]
- **Status**: [ ] Proposto [ ] Em Execução [ ] Completo
- **Link Workana**: 
- **Descrição**: 
- **Valor**: R$ 
- **Tech Usada**: 
- **Resultado**: 
- **Feedback do Cliente**: 

---

## 🔗 Recursos & Links Importantes

### Documentação Oficial
- [Claude Documentation](https://docs.claude.com)
- [Hugging Face Course](https://huggingface.co/learn)
- [DeepLearning.AI](https://www.deeplearning.ai)

### Ferramentas & Plataformas
- [LangChain](https://www.langchain.com)
- [Pinecone](https://www.pinecone.io) (Vector DB)
- [Anthropic Claude API](https://claude.ai)
- [Kaggle](https://www.kaggle.com)
- [Google Colab](https://colab.research.google.com)

### Papers & Pesquisa
- [ArXiv - AI Papers](https://arxiv.org)
- [Anthropic Research](https://www.anthropic.com/research)

### Comunidades
- [GitHub](https://github.com)
- [Stack Overflow](https://stackoverflow.com)
- [Dev.to](https://dev.to)

---

## 📝 Notas Pessoais

### O Que Está Funcionando Bem
- (Você vai anotando o que aprova)

### Desafios Atuais
- (Você vai anotando dificuldades)

### Ideias de Projetos Futuros
- (Você vai brainstormando)

---

## 🎓 Conhecimento Adquirido por Tema

### LLMs & Prompting
- Status: ✅ Fundamentos + Prompt Engineering concluídos (Semanas 1-2)
- Confiança: 6/10
- Próximo: Projeto Prático LLMs (Semana 3) — candidato forte: migração Delphi → Java Spring Boot

### RAG
- Status: ⏳ Não iniciado
- Confiança: 0/10

### Agentes
- Status: ⏳ Não iniciado
- Confiança: 0/10

### MCP
- Status: ✅ Conhecimento Base
- Confiança: 3/10
- Nota: Já estudou conceitos, precisa praticar

---

## 📊 Estatísticas de Progresso

```
Semanas Completadas: 2/16
Projetos Completos: 0
Projetos em Progresso: 0
Horas Totais: ~2h / ~200h
Freelancers Completados: 0
```

---

## 🔔 Próximas Ações

**SEMANA 1 (CONCLUÍDA)**:
1. [x] Estudar Transformers (2h)
2. [x] Fazer 1ª API call (1h)
3. [x] Entender tokens e contexto (1h)
4. [ ] Fazer 1º projeto (4h) — não obrigatório na Semana 1 conforme currículo original; mover para Semana 3 se aplicável

**SEMANA 2 (CONCLUÍDA)**:
1. [x] Estudar few-shot, CoT, role prompting, structured output, temperature/top_p
2. [x] Estudar segurança em prompts / prompt injection (extra)
3. [x] Criar 6 prompts aplicados (5 do currículo + migração de linguagem)

**Após Semana 2**:
1. [x] Atualizar este arquivo
2. [ ] Fazer push ao GitHub
3. [ ] Preparar Semana 3 (projeto prático — considerar migração Delphi → Java Spring Boot)

---

## 📞 Contato com Mentores/Consultores

- Claude (IA): Disponível 24/7
- Seu Email: marloncdrodrigues@gmail.com

---

**Última Atualização**: 28/08/2026 (Semana 2 concluída)  
**Próxima Revisão**: 09/09/2026

---

### Como Usar Este Arquivo

1. **Atualize semanalmente** - Toda segunda-feira ou quando terminar semana
2. **Marque progresso** - Use [ ] para incompleto, [x] para completo
3. **Adicione código** - Coloque seus exemplos nas seções específicas
4. **Documente aprendizados** - Sempre anote o que aprendeu
5. **Compartilhe com novo agente** - Se trocar de assistente, passe este arquivo

---

**🎯 VOCÊ CONSEGUE! Vamos lá! 🚀**
