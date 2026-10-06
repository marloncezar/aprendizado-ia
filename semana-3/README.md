# Semana 3 — Projeto prático: conduzir histórias do `elo_api` com LLM

> Mudança de escopo: o projeto original (migração Delphi → Java) foi substituído.
> O `elo_api` (Novo ELO, IMA) é um **sistema novo em Rails**; o legado Delphi é só fonte de regra de negócio (já consolidada nos ADRs).

## Objetivo
Usar o fluxo **planejar → aprovar → executar** das Diretrizes IMA (Claude Code, sem API própria) para entregar histórias da Fase 0 do backlog:
- ~~F0-05 e F0-06~~: o time já havia entregue; trocadas pela **F1-06** — papéis locais, permissões globais, vínculo de papel e de UG (**concluída**)
- (F0-03, matriz de versões, já definida pelo time — ver `ambiente.md`)

## Regras de segurança (código da IMA não é meu)
- Código e docs do `elo_api` ficam em `~/Documentos/ia/elo_api`, **fora** deste repositório. Nunca commitar aqui.
- Não enviar conteúdo do `elo_api` a APIs/tiers que treinem com os dados (ex.: Gemini gratuito). Confirmar com o time antes de usar qualquer API externa.
- Entregas do projeto vão por branch + merge request no GitLab da IMA.
- Neste repo entram só: planos, prompts e aprendizados (sem trechos do código/docs da IMA).

## Fluxo por história
1. **Planejar** (sem editar arquivos): prompt com CoT → perguntas, plano, critérios de aceite, riscos, fora de escopo. Salvar em `planos/`.
2. **Aprovar** o plano (revisão humana).
3. **Executar** em mudanças pequenas, teste primeiro (SQL/rspec).
4. **Verificar** o critério de aceite do backlog; registrar o que o LLM errou.
5. **Registrar** aprendizados em `PROGRESSO.md`.

## Estrutura
- `ambiente.md` — versões-alvo e como configurar a máquina
- `prompts/` — prompts de planejamento/execução reutilizáveis
- `planos/` — plano de cada história

## Critério de pronto
**Atingido (06/10/2026):** F1-06 entregue com testes, publicada no GitLab da IMA (`mrodrigues` e `desenvolvimento`), com registro de prompts (`prompts/`) e dos erros do LLM (`PROGRESSO.md`). O merge foi direto em `desenvolvimento`; não houve MR formal.
