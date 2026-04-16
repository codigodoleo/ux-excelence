---
name: ux-excelence:review-visual
description: Revisão visual — espaçamento, proporção, hierarquia e whitespace. Deriva critérios das fontes de verdade do projecto.
user-invocable: true
---

# Review Visual

## Fontes de verdade a procurar

Usar Glob para encontrar no projecto (por nome, qualquer caminho):
1. `**/design-system.lib.pen` → tokens de espaçamento, grid, gap
2. `**/website-brief.md` → comportamento visual esperado (whitespace, blocos amplos, imagens)
3. `**/brand-dna.md` → território visual, sensações obrigatórias

Se algum ficheiro não for encontrado: registar ausência no relatório e continuar com os restantes.

## Processo de execução

### PASSO 1 — DESCOBERTA
- Verificar com `get_editor_state` que o documento activo é `homepage.pen`; se não for, alertar o utilizador e pausar
- Usar Glob para localizar cada ficheiro de fonte de verdade no projecto antes de o ler
- Ler website-brief.md → extrair: regras de comportamento visual, o que evitar
- Usar `get_variables` no documento activo para verificar se existem tokens de espaçamento; se não existirem (é comum — os tokens são maioritariamente cor e tipografia), derivar os valores de espaçamento base a partir das descrições do `website-brief.md` (ex: "blocos amplos", "muito espaço em branco") e usar `snapshot_layout` nas primeiras seções para estabelecer os valores reais como baseline
- Registar critérios de espaçamento antes de prosseguir

### PASSO 2 — ANÁLISE (seção a seção)
Para cada seção da homepage (obter lista com get_editor_state):
- Screenshot com get_screenshot
- snapshot_layout com maxDepth:2 para medir espaçamentos reais
- Verificar:
  - [ ] Padding lateral consistente em todas as seções (≥ valor definido no design-system)
  - [ ] Padding vertical (top/bottom) proporcional ao peso da seção
  - [ ] Gap entre eyebrow e título: consistente entre seções equivalentes
  - [ ] Gap entre título e subtítulo: consistente
  - [ ] Gap entre subtítulo e CTA: consistente
  - [ ] Imagens com peso visual adequado (não comprimidas, não cortadas)
  - [ ] Whitespace suficiente — sem blocos colados
  - [ ] Hierarquia visual clara: elemento principal destaca-se dos secundários
  - [ ] Colunas alinhadas ao mesmo grid entre seções comparáveis

### PASSO 3 — CORRECÇÃO
Para cada desvio encontrado:
1. Apresentar ao utilizador: seção, valor actual vs valor esperado, fonte do critério
2. Aguardar aprovação
3b. Verificar com `get_editor_state` que o ficheiro activo ainda é `homepage.pen` antes de aplicar
4. Aplicar correcção no Pencil (U() com padding/gap/width correcto)
5. Screenshot de verificação
6. Confirmar correcção

### PASSO 4 — RELATÓRIO

```
SKILL: review-visual
FONTES LIDAS: [lista]
CRITÉRIOS DE ESPAÇAMENTO EXTRAÍDOS: [valores base]
---
[ID] [Nome]   ✅ aprovado
[ID] [Nome]   ⚠️ corrigido — padding lateral 64→80px [fonte: design-system.lib.pen]
[ID] [Nome]   ❌ pendente — [motivo]
---
RESUMO: X aprovadas · Y corrigidas · Z pendentes
```

## Regras de sessão
- NUNCA trocar de ficheiro .pen durante a execução
- NUNCA corrigir sem aprovação do utilizador
- Usar sempre snapshot_layout para medir — nunca estimar valores
- Manter homepage.pen como documento activo durante toda a skill
- Citar sempre a fonte (ficheiro + secção) de cada critério aplicado
