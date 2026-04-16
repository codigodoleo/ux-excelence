---
name: ux-excelence:review-ux
description: Revisão de UX — fluxo de conversão, hierarquia de CTAs e canais de contacto. Deriva critérios do website-brief do projecto.
user-invocable: true
---

# Review de UX

## Fontes de verdade a procurar

Usar Glob para encontrar no projecto (por nome, qualquer caminho):
1. `**/website-brief.md` → objectivos de conversão, públicos, CTAs principais, sitemap
2. `**/components.lib.pen` → hierarquia de botões disponíveis (Primary, Secondary, Outline, Link)
3. `**/brand-dna.md` → canais de contacto preferidos, tom do CTA

Se algum ficheiro não for encontrado: registar ausência no relatório e continuar com os restantes.

## Processo de execução

### PASSO 1 — DESCOBERTA
- Verificar com `get_editor_state` que o documento activo é `homepage.pen`; se não for, alertar o utilizador e pausar
- Usar Glob para localizar cada ficheiro de fonte de verdade no projecto antes de o ler
- Ler website-brief.md → extrair:
  - Objectivo de conversão por público (cliente final vs arquitetos)
  - CTAs principais definidos
  - Canais de conversão desejados (WhatsApp, formulário, etc.)
- Usar `batch_get` em `components.lib.pen` com `patterns:[{type:"frame", nameContains:"Btn"}]` para identificar os botões disponíveis e a sua hierarquia (Primary, Secondary, Outline, Link) — sem fazer `open_document` (nunca trocar o ficheiro activo)
- Registar critérios antes de prosseguir

### PASSO 2 — ANÁLISE (seção a seção)
Para cada seção da homepage (obter lista com get_editor_state):
- Screenshot com get_screenshot
- snapshot_layout para identificar elementos interactivos
- Verificar:
  - [ ] Máximo um CTA primário por seção (sem competição)
  - [ ] Hierarquia de botões correcta: Primary > Secondary/Link (não dois Primary na mesma seção)
  - [ ] Canal B2B (arquitetos) sinalizado — verificar presença em ≥ seções definidas no brief
  - [ ] Canal B2C (cliente final) presente no hero e no CTA final
  - [ ] Contacto WhatsApp acessível (footer ou floating)
  - [ ] CTAs usam componentes da lib (não botões manuais)
  - [ ] Seções com objectivo de conversão têm CTA visível
  - [ ] Sem dead-ends (seção sem nenhuma acção possível)

### PASSO 3 — CORRECÇÃO
Para cada desvio encontrado:
1. Apresentar ao utilizador: seção, problema de UX, correcção proposta, fonte do critério
2. Aguardar aprovação
3b. Verificar com `get_editor_state` que o ficheiro activo ainda é `homepage.pen` antes de aplicar
4. Aplicar no Pencil (substituir botão manual por instância da lib, ajustar hierarquia)
5. Screenshot de verificação
6. Confirmar correcção

### PASSO 4 — RELATÓRIO

```
SKILL: review-ux
FONTES LIDAS: [lista]
OBJECTIVOS DE CONVERSÃO EXTRAÍDOS: [B2C: ... / B2B: ...]
CTAs DEFINIDOS NO BRIEF: [lista]
---
[ID] [Nome]   ✅ aprovado
[ID] [Nome]   ⚠️ corrigido — dois Btn/Primary → Primary + Secondary [fonte: website-brief.md]
[ID] [Nome]   ❌ pendente — [motivo]
---
RESUMO: X aprovadas · Y corrigidas · Z pendentes
```

## Regras de sessão
- NUNCA trocar de ficheiro .pen durante a execução
- NUNCA corrigir sem aprovação do utilizador
- Avaliar UX do ponto de vista do utilizador final — não do designer
- Manter homepage.pen como documento activo durante toda a skill
- Citar sempre a fonte (ficheiro + secção) de cada critério aplicado
