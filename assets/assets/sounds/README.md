💡 Por que essa combinação é a melhor para Android?
Economia de RAM no Dispositivo:
Sons carregados com preloadSound() ficam na memória RAM da GPU/CPU. Converter 20 sons de efeitos de Stereo 320kbps para Mono 96kbps reduz o consumo de memória de ~30 MB para menos de 4 MB no Android.

Impacto Mínimo no tamanho do APK:
O compressor Vorbis do OGG mantém o tamanho do arquivo ultra-compacto, o que ajuda a manter seu APK/AAB abaixo dos limites da Google Play Store.

Evita o Resampling da AAudio/OpenSL (Driver do Android):
A maioria dos chips de áudio no Android trabalha nativamente em 44100 Hz. Manter essa taxa no arquivo evita que o sistema do celular gaste ciclos de CPU reamostrando o áudio em tempo real.

---
Para garantir o equilíbrio ideal entre tamanho do APK, tempo de carregamento na Web, uso de memória RAM no Android e qualidade sonora, a regra de ouro no flutter_soloud é a estratégia híbrida usando OGG Vorbis em 44.1 kHz.

O formato OGG é nativo do SoLoud (processado direto em C++), não sofre com o problema de silêncio no loop do MP3 e tem um tamanho até 80% menor que o WAV.


1. Converter todos os Efeitos Sonoros para OGG Mono (Qualidade + Alta Perf):

NA PASTA ONDE ESTAO OS ARQUIVOS

```sh
brew install ffmpeg && for f in *.{wav,mp3}; do [ -f "$f" ] && ffmpeg -i "$f" -vn -ar 44100 -ac 1 -b:a 96k -c:a libvorbis "${f%.*}.ogg"; done
```

2. Converter todas as Músicas de Fundo para OGG Stereo (Loop Perfeito):

```sh
for f in *.wav; do npx ffmpeg-cli -i "$f" -vn -ar 44100 -ac 2 -b:a 128k -c:a libvorbis "${f%.*}.ogg"; done
```