---
name: ux-excelence:review-brand
description: Revisão de marca — paleta, tokens, tipografia e logo. Procura fontes de verdade no projecto e deriva critérios a partir delas.
user-invocable: true
---

# Review de Marca

## Fontes de verdade a procurar

Usar Glob para encontrar no projecto (por nome, qualquer caminho):
1. `**/design-system.lib.pen` → tokens de cor, tipografia, variáveis
2. `**/components.lib.pen` → componentes canónicos
3. `**/brand-dna.md` → personalidade, território, cores de identidade, regras de uso
4. `**/briefing-logo.md` → versões do logo, regras de contexto claro/escuro
5. `**/website-brief.md` → tom de voz, públicos, posicionamento

Se algum ficheiro não for encontrado: registar ausência no relatório e continuar com os restantes.

## Processo de execução

### PASSO 1 — DESCOBERTA
- Verificar com `get_editor_state` que o documento activo é `homepage.pen`; se não for, alertar o utilizador e pausar
- Usar Glob para localizar cada ficheiro de fonte de verdade
- Ler brand-dna.md → extrair: cores de identidade, regras de proporção, cores proibidas
- Ler website-brief.md → extrair: paleta definida, regras tipográficas
- Ler briefing-logo.md → extrair: versões do logo, contextos de uso
- Usar `get_variables` no documento activo para ler os tokens disponíveis (os tokens do design-system devem estar importados na homepage); se ausentes, usar `batch_get` em `design-system.lib.pen` com `properties:["variables"]` — sem fazer `open_document` (nunca trocar o ficheiro activo)
- Registar todos os critérios extraídos antes de prosseguir

### PASSO 2 — ANÁLISE (seção a seção)
Para cada seção da homepage (obter lista com get_editor_state):
- Screenshot com get_screenshot
- `batch_get` dos nós da seção com `properties:["fill","stroke","color"]` para detectar cores hard-coded (não-token)
- Verificar:
  - [ ] Fundo usa token do design-system (não cor hard-coded)
  - [ ] Cor de identidade primária está presente
  - [ ] Alternância de fundos entre seções consecutivas
  - [ ] Título usa fonte serif definida no brand-dna
  - [ ] Corpo de texto usa fonte sans definida no brand-dna
  - [ ] Eyebrows usam cor de acento definida (não grafite sobre fundo claro, não muted sobre fundo escuro)
  - [ ] Logo usa versão correcta para o fundo (claro/escuro)
  - [ ] Sem cores hard-coded fora dos tokens do design-system
  - [ ] Cor de acento/bronze não domina — usada apenas como detalhe

### PASSO 3 — CORRECÇÃO
Para cada desvio encontrado:
1. Apresentar ao utilizador: seção, desvio, correcção proposta, fonte do critério
2. Aguardar aprovação
3b. Verificar com `get_editor_state` que o ficheiro activo ainda é `homepage.pen` antes de aplicar
4. Aplicar correcção no Pencil (batch_design com U() operation)
5. Screenshot de verificação
6. Confirmar correcção

### PASSO 4 — RELATÓRIO
Emitir relatório no formato:

```
SKILL: review-brand
FONTES LIDAS: [lista dos ficheiros encontrados]
CRITÉRIOS EXTRAÍDOS: [resumo dos critérios derivados]
---
[ID] [Nome da Seção]   ✅ aprovado
[ID] [Nome da Seção]   ⚠️ corrigido — [desvio] → [correcção] [fonte: ficheiro]
[ID] [Nome da Seção]   ❌ pendente — [motivo]
---
RESUMO: X aprovadas · Y corrigidas · Z pendentes
```

## Regras de sessão
- NUNCA trocar de ficheiro .pen durante a execução
- NUNCA corrigir sem aprovação do utilizador
- Citar sempre a fonte (ficheiro + secção) de cada critério aplicado
- Manter homepage.pen como documento activo durante toda a skill
