# Prompt de revisão adversarial de autorização

Técnica: pedir ao LLM para atacar o próprio resultado. Complementar com mutação manual (abaixo).

```
Revise o diff como um atacante que quer ler dados de outro cliente ou ganhar permissão.
Para cada endpoint e serviço novo responda, com evidência no código:
1. De onde vem o cliente? Há algum caminho em que um parâmetro o substitua?
2. O que acontece com ID de outro cliente, ID inexistente e ID malformado? Devem ser
   indistinguíveis (404).
3. A permissão é lida do banco a cada requisição? Existe cache ou claim de papel no token?
4. Estado global ou de thread (CurrentAttributes, variável de classe) usado em segurança?
5. Duas requisições simultâneas quebram alguma invariante (ex.: último administrador)?
6. O que o papel de banco usado consegue fazer além do necessário?
Liste achados por severidade; se não achar nada, diga o que verificou.
```

## Mutação manual (o LLM não faz por você)
Quebre de propósito uma proteção e confirme que algum teste falha; restaure em seguida.
- Policy sempre permitindo → os testes de 403 devem falhar.
- Remover a definição do contexto do cliente → todos os testes de request devem falhar.
- Remover a trava de linha antes de contar → o teste de concorrência deve falhar.
Teste que passa com a proteção quebrada não prova nada.
