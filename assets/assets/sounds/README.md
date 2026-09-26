# 🎵 Arquivos de Áudio - Memory Master

Esta pasta contém os arquivos de áudio (efeitos sonoros) usados no jogo.

## Sites para baixar

https://sounddino.com/pt/effects/winning/

## 📋 Arquivos Necessários

Para que os efeitos de áudio funcionem, você precisa adicionar os seguintes arquivos nesta pasta:

### Sons de Match (4 arquivos)

1. **match_full_lucky.mp3** - Som para "Lucky Shot" (🎲)
   - Som especial e comemorativo para quando acerta por sorte total
   - Sugestão: Som de celebração, "whoosh" ou "ding" especial

2. **match_lucky.mp3** - Som para "Got Lucky" (🍀)
   - Som positivo para quando acerta parcialmente por sorte
   - Sugestão: Som de sucesso positivo, "ping" ou "chime"

3. **match_perfect.mp3** - Som para "Perfect" (✨)
   - Som mais satisfatório para quando demonstra memorização perfeita
   - Sugestão: Som de sucesso premium, "sparkle" ou "magic"

4. **match_normal.mp3** - Som para match normal (🎯)
   - Som padrão para matches normais
   - Sugestão: Som de confirmação simples, "click" ou "pop"

### Outros Sons

5. **victory.mp3** - Som de vitória quando completa o jogo
   - Som de comemoração mais longo e satisfatório
   - Sugestão: Fanfarra curta, "success" ou "celebration"

6. **error.mp3** - Som de erro quando faz match errado
   - Som curto e discreto para feedback negativo
   - Sugestão: "buzz" curto, "error" ou "wrong"

7. **flip.mp3** - Som quando vira uma carta
   - Som curto e discreto para feedback tátil
   - Sugestão: "flip" curto, "card" ou "page turn"

## 🎨 Características Recomendadas

- **Formato**: MP3 (compatível com todas as plataformas)
- **Duração**: 
  - Match sounds: 0.3-0.8 segundos
  - Victory: 1-3 segundos
  - Error/Flip: 0.1-0.3 segundos
- **Volume**: Moderado (não muito alto para não irritar)
- **Qualidade**: 44.1kHz, 128kbps é suficiente

## 📚 Onde Encontrar Sons Gratuitos

Alguns sites recomendados para encontrar sons gratuitos:

- **Freesound.org** - Biblioteca enorme de sons gratuitos (requer login)
- **Zapsplat.com** - Sons gratuitos de alta qualidade (requer login)
- **Mixkit.co** - Sons gratuitos sem necessidade de login
- **OpenGameArt.org** - Recursos de áudio para jogos
- **Kenney.nl** - Assets gratuitos incluindo sons

## 🔍 Termos de Busca Sugeridos

- "success sound"
- "match sound"
- "card flip"
- "celebration"
- "error sound"
- "game sound effect"

## ⚠️ Nota Importante

Se os arquivos de áudio não estiverem presentes, o jogo continuará funcionando normalmente.
Os erros serão silenciosamente ignorados (apenas em modo debug será logado).

Para desabilitar completamente os sons, use `AudioService.setEnabled(false)`.
