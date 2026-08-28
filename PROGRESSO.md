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
| **Semana Atual** | 1 |
| **Horas Investidas** | ~2h |
| **Meta Semanal** | 5-15h (seu ritmo) |
| **Timeline Total** | 16 semanas |
| **Próximo Milestone** | Semana 4 - Primeiro Freelancer |

**Status do Repositório**: 🔄 Semana 1 concluída

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
**Status**: ⏳ Planejado  
**Tempo Planejado**: 8h

**Objetivos**:
- [ ] Few-shot learning
- [ ] Chain-of-thought prompting
- [ ] Role prompting (dar "personagem" ao LLM)
- [ ] Structured output (JSON, XML)
- [ ] Temperature e top_p

**Projeto**: Criar 5 prompts para seu trabalho de dev
- Exemplo 1: Gerar boilerplate
- Exemplo 2: Refatorar código
- Exemplo 3: Escrever documentação
- Exemplo 4: Gerar testes
- Exemplo 5: Code review assistido

**Recursos**:
- DeepLearning.AI - Prompting Course
- Anthropic Research Papers

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
- [ ] Outro (qual?): 

**Resultado**: Solução pronta + documentação + código no GitHub

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
- Status: 🔄 Fundamentos concluídos (Semana 1)
- Confiança: 4/10
- Próximo: Prompt Engineering (Semana 2)

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
Semanas Completadas: 1/16
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

**Após Semana 1**:
1. [x] Atualizar este arquivo
2. [ ] Fazer push ao GitHub
3. [ ] Preparar Semana 2

---

## 📞 Contato com Mentores/Consultores

- Claude (IA): Disponível 24/7
- Seu Email: marloncdrodrigues@gmail.com

---

**Última Atualização**: 27/08/2026 (Semana 1 concluída)  
**Próxima Revisão**: 02/09/2026

---

### Como Usar Este Arquivo

1. **Atualize semanalmente** - Toda segunda-feira ou quando terminar semana
2. **Marque progresso** - Use [ ] para incompleto, [x] para completo
3. **Adicione código** - Coloque seus exemplos nas seções específicas
4. **Documente aprendizados** - Sempre anote o que aprendeu
5. **Compartilhe com novo agente** - Se trocar de assistente, passe este arquivo

---

**🎯 VOCÊ CONSEGUE! Vamos lá! 🚀**
