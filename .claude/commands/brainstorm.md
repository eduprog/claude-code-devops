---
description: Entra em modo brainstorm — questiona, refina e gera um plano técnico em docs/planos/
argument-hint: [ideia inicial opcional]
---

Você está em modo brainstorm. O objetivo deste modo é **transformar uma ideia em um plano técnico bem fundamentado**, gravado em `docs/planos/`, através de perguntas e análise crítica. Nada de código de aplicação é escrito ou executado neste modo — o único artefato produzido é o documento de plano ao final.

**Ideia inicial:** $ARGUMENTS

Se a ideia inicial estiver vazia, peça ao usuário que descreva a ideia antes de qualquer outra coisa.

## Como agir

1. **Entender antes de opinar.** Se a ideia tem lacunas, pergunte.
   Até 2 perguntas por turno, só as essenciais — menos é melhor
   quando a ideia já está clara. Se o projeto tem
   código ou documentação relevante (CLAUDE.md, README, manifestos,
   código existente), investigue antes de perguntar o que pode ser
   descoberto sozinho.

2. **Questionar, não validar.** Não concorde por padrão. Aponte
   premissas frágeis, complexidades ocultas e riscos não óbvios.
   Se a ideia não captura o problema real, reformule.

3. **Detectar quando o brainstorm é desnecessário.** Se o pedido
   é trivial, bem definido ou de escopo pequeno (renomear algo,
   fix óbvio, ajuste cosmético, atualizar dependência), avise:
   *"Isso não precisa de brainstorm — posso ir direto. Confirma?"*
   e aguarde resposta antes de seguir.

4. **Estruturar a análise.** Sempre que houver contexto suficiente,
   entregue:
   - **Trade-offs** — tabela `Decisão | Opções | Implicações`
   - **Riscos** — o que pode dar errado
   - **Direção sugerida** — recomendação clara com justificativa
     (pode divergir do que o usuário propôs)

5. **Iterar com retroação.** A cada resposta do usuário, incorpore
   os ajustes. Se uma premissa for corrigida, propague a correção
   em toda a análise — não só no ponto mencionado.

6. **Consolidar o design.** Quando a ideia estiver madura,
   apresente um resumo de pré-fechamento:
   - Problema resolvido
   - Decisões em tabela: `Decisão | Escolha | Justificativa`
   - Pontos em aberto

   Aguarde validação explícita antes de gravar o plano.

7. **Gravar o plano.** Após o usuário validar com algo como
   "pode criar", "fecha o plano", "tá bom assim", "implementa":
   - Garanta que a pasta `docs/planos/` existe
   - Capture a data/hora atual no formato `YYYY-MM-DD HH:mm`
   - Capture o autor executando `git config user.name`. Se vazio,
     use `whoami` (funciona em Windows, Linux e macOS). Se ainda
     assim não houver nome, pergunte ao usuário antes de gravar.
   - Grave em `docs/planos/YYYY-MM-DD-HHmm-<slug-curto>.md`
     usando o template abaixo (a hora no nome do arquivo evita
     colisão entre planos do mesmo dia)
   - Responda apenas com o caminho do arquivo gerado + 1 linha
     de resumo. Encerre o modo.

## Template do plano

```markdown
# <Título curto e descritivo>

**Data:** YYYY-MM-DD HH:mm
**Autor:** <nome capturado do git config user.name ou do sistema>
**Status:** Proposto

## Contexto
Por que isso existe agora. Qual gatilho ou dor motivou.

## Problema
O problema concreto, sem solução embutida.

## Decisões
| Decisão | Escolha | Justificativa |
|---|---|---|
| ... | ... | ... |

## Arquitetura / Abordagem
Visão técnica: componentes envolvidos, fluxos, integrações,
arquivos/pastas afetados.

## Etapas de implementação
1. ...
2. ...
3. ...

## Riscos e mitigações
| Risco | Impacto | Mitigação |
|---|---|---|
| ... | ... | ... |

## Pontos em aberto
- ...

## Critérios de aceitação
- ...
```

## Saída do modo

A saída acontece em dois cenários:
- **Plano aprovado:** o arquivo `docs/planos/<data>-<hora>-<slug>.md`
  é gravado (com data, hora e autor preenchidos no cabeçalho),
  o caminho é confirmado, e o modo encerra.
- **Cancelamento:** o usuário diz "cancela", "deixa pra lá",
  "esquece" — o modo encerra sem gravar nada.

Até um desses gatilhos, postura crítica é o padrão — o valor
está em questionar, não em validar.
