---
name: ux-excelence:review-copy
description: Revisão de copy — tom, vocabulário, CTAs e headlines. Deriva critérios do brand-dna e website-brief do projecto.
user-invocable: true
---

# Review de Copy

## Fontes de verdade a procurar

Usar Glob para encontrar no projecto (por nome, qualquer caminho):
1. `**/brand-dna.md` → tom de voz, palavras obrigatórias, palavras proibidas
2. `**/website-brief.md` → copy recomendado por seção, CTAs definidos, vocabulário da marca
3. `**/briefing-logo.md` → nome oficial da marca, tagline

Se algum ficheiro não for encontrado: registar ausência no relatório e continuar com os restantes.

## Processo de execução

### PASSO 1 — DESCOBERTA
- Verificar com `get_editor_state` que o documento activo é `homepage.pen`; se não for, alertar o utilizador e pausar
- Usar Glob para localizar cada ficheiro de fonte de verdade no projecto antes de o ler
- Ler brand-dna.md → extrair:
  - Lista de palavras/conceitos obrigatórios
  - Lista de palavras/expressões proibidas
  - Tom de voz (adjectivos que definem como a marca escreve)
  - Tom de voz negativo (o que a marca nunca deve soar)
- Ler website-brief.md → extrair:
  - CTAs definidos para cada público (cliente final, arquitetos)
  - Headlines aprovadas por seção
  - Vocabulário específico da marca
- Registar critérios antes de prosseguir

### PASSO 2 — ANÁLISE (seção a seção)
Para cada seção da homepage (obter lista com get_editor_state):
- Screenshot com get_screenshot
- Usar `batch_get` para ler os nós de texto da seção com `patterns:[{type:"text"}]` e `properties:["content","style"]`
- Verificar para cada texto:
  - [ ] Vocabulário de marca presente (palavras obrigatórias das fontes)
  - [ ] Sem linguagem promocional ou urgência artificial
  - [ ] Sem termos genéricos sinalizados nas fontes
  - [ ] Tom alinhado com os adjectivos extraídos do brand-dna
  - [ ] CTAs usam as formulações definidas no website-brief
  - [ ] Headlines têm força e especificidade (não vagueza)
  - [ ] Nome da marca escrito correctamente (conforme briefing-logo)
  - [ ] Tagline presente onde aplicável

### PASSO 3 — CORRECÇÃO
Para cada desvio encontrado:
1. Apresentar ao utilizador: seção, texto actual, problema identificado, alternativa proposta, fonte do critério
2. Aguardar aprovação do texto alternativo
3b. Verificar com `get_editor_state` que o ficheiro activo ainda é `homepage.pen` antes de aplicar
4. Aplicar no Pencil via batch_design (U() com content)
5. Screenshot de verificação
6. Confirmar correcção

### PASSO 4 — RELATÓRIO

```
SKILL: review-copy
FONTES LIDAS: [lista]
TOM EXTRAÍDO: [adjectivos positivos / negativos]
VOCABULÁRIO OBRIGATÓRIO: [palavras encontradas nas fontes]
---
[ID] [Nome]   ✅ aprovado
[ID] [Nome]   ⚠️ corrigido — "experiência única" → "curadoria próxima" [fonte: brand-dna.md]
[ID] [Nome]   ❌ pendente — [motivo]
---
RESUMO: X aprovadas · Y corrigidas · Z pendentes
```

## Regras de sessão
- NUNCA trocar de ficheiro .pen durante a execução
- NUNCA alterar copy sem apresentar alternativa e aguardar aprovação
- Citar sempre a fonte (ficheiro + secção) de cada critério aplicado
- Manter o tom proposto — não inventar copy fora das directrizes das fontes
- Manter homepage.pen como documento activo durante toda a skill
