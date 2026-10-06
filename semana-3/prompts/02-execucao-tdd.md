# Prompt de execução (teste primeiro, passo a passo)

Técnicas: decomposição, few-shot implícito (padrão do código vizinho), restrições explícitas, verificação por comando.

```
Plano aprovado: [cole o plano]. Execute SOMENTE o passo [N], em uma branch de trabalho.

Regras:
- Escreva o teste ANTES e mostre que ele falha pelo motivo certo.
- Siga o estilo do código vizinho (nomes, comentários, idioma); não invente padrão novo.
- Mudança mínima: nada de refatorar o que não é do passo.
- Segurança: o cliente vem do token, nunca de parâmetro; sem segredo em log nem commit;
  privilégio de banco mínimo, com teste do que NÃO pode.
- Ao terminar: rode a suíte inteira e os linters e cole a saída. Se algo falhar, diga,
  não esconda.
- Um commit por passo, mensagem em português explicando o porquê.
Pare e pergunte se o passo exigir uma decisão que não está no plano.
```
