# Revisão rápida da base de código

## 1) Tarefa de correção de erro de digitação
**Problema identificado:** inconsistência de grafia/capitalização da marca.
- O título e o cabeçalho exibem `UltraCell`.
- O texto de marca d'água usa `Ultracell`.

**Tarefa sugerida:** padronizar a escrita da marca em todo o código (`UltraCell`) e criar uma constante única reutilizável para evitar divergências futuras.

## 2) Tarefa de correção de bug
**Problema identificado:** função `mountLocalFontFaces()` está vazia.
- Há catálogo de fontes locais com `isLocal: true`.
- Se a vetorização com OpenType falhar para alguma fonte, o fallback visual pode não carregar corretamente no preview porque não há registro explícito de `@font-face` em runtime.

**Tarefa sugerida:** implementar `mountLocalFontFaces()` para registrar dinamicamente as fontes locais via `FontFace` + `document.fonts.add`, consumindo os dados de `window.__FONT_DATA__`.

## 3) Tarefa de ajuste de documentação
**Problema identificado:** README está incompleto para o escopo atual do projeto.
- Só contém nome do projeto e uma frase curta.
- Não documenta dependências externas (jsPDF, opentype.js, svg2pdf), fluxo de compartilhamento, nem limitações conhecidas.

**Tarefa sugerida:** atualizar o `README.md` com:
1. descrição funcional da aplicação,
2. instruções de execução local,
3. dependências e papel de cada biblioteca,
4. limitações (ex.: comportamento de compartilhamento por plataforma),
5. estratégia de fontes locais (font-data / fallback / vetorização).

## 4) Tarefa para melhorar testes
**Problema identificado:** não há suíte de testes automatizados para regras críticas de render/export.

**Tarefa sugerida:** criar testes unitários (ex.: Vitest) para funções puras e comportamentos de borda:
- `slugify` (acentos, espaços, símbolos),
- `hexToRgb`/`rgbToHex`/`mixHex` (entradas válidas e inválidas),
- `getPreviewFitConfig` (nomes curtos/longos),
- `getStatusText` (fontes locais/remotas disponíveis ou não).

Como etapa seguinte, adicionar um teste de integração leve para garantir que `updateAll()` renderize previews para todos os cards sem erro quando `window.__FONT_DATA__` estiver presente e quando estiver ausente.
