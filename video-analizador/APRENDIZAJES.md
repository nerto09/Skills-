# Aprendizajes y preferencias: transcripción/análisis de video
Archivo vivo. Se lee antes de cada solicitud y se enriquece después. Objetivo: que el usuario escriba lo mínimo.

## Preferencias del usuario (defaults; no volver a preguntar)
- Idioma principal: **español**; permitir inglés/otros.
- Entrega: TXT, JSON, SRT/VTT e informe combinado (AUDIO / TEXTO EN PANTALLA / TIEMPO).
- Formatos de entrada: MP4, MOV, AVI, MKV, WEBM.
- Herramientas: preferir GitHub oficial, PyPI y apt; nada de sitios desconocidos.
- Comunica en español, breve, sin preámbulos.

## Solicitud mínima (plantilla)
> "Transcribe y analiza <ruta o adjunto del video>"
Todo lo demás se asume por defecto (ver arriba). Solo se añade si cambia algo:
`idioma=en`, `modelo=medium`, `intervalo_ocr=2`, `solo_audio`, `solo_ocr`.

## Flujo estándar
1. `video_analizador <video> -o <carpeta>` (ver SKILL.md).
2. Revisar `*_informe.txt` y comprobar `estado_audio` / `estado_ocr` en el JSON.
3. Entregar la ruta exacta de los archivos generados.

## Lecciones técnicas
- **Red del entorno cloud:** apt y PyPI funcionan; `huggingface.co` y GitHub (archivos) están bloqueados → los pesos de Whisper no se descargan. Solución: permitir `huggingface.co` o usar `--model /ruta/local`. Ejecutar con `--offline` evita esperas.
- **apt:** si falla con 404, ejecutar `apt-get update` antes de instalar.
- **Sin modelo Whisper** el script no falla: marca tramos de voz con Silero VAD y lo indica en el informe (`SIN_TRANSCRIPCION`).
- **OCR español:** `tesseract-ocr-spa` + `--oem 1 --psm 11`, ampliando el fotograma a ≥1600 px; tildes y ñ salen bien. Umbral de confianza 60.
- **Duplicados OCR:** los textos iguales en fotogramas consecutivos se fusionan (similitud ≥0.85) en un solo rango de tiempo.
- **Modelos y peso:** small ~470 MB, medium ~1,5 GB, large-v3 ~3 GB; CPU basta (int8), GPU acelera.
- Pendiente de validar con voz real: precisión del español y elección de modelo.

## Registro de casos (añadir una línea por trabajo)
| Fecha | Video | Resultado | Aprendizaje |
|---|---|---|---|
| 2026-09-29 | ejemplo sintético 6 s | OCR OK, ASR bloqueado por red | ver "Lecciones técnicas" |
