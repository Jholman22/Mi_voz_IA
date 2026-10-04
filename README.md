# 🎙️ Clonación de voz con IA (XTTS-v2 + Google Colab)

Sistema de síntesis de voz personalizada: escribes un texto largo y obtienes el audio con **tu propia voz**. Se entrena (fine-tuning) sobre el modelo [XTTS-v2](https://huggingface.co/coqui/XTTS-v2) usando unos 20 minutos de tus grabaciones, todo en una GPU gratuita de Google Colab.

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/TU_USUARIO/TU_REPOSITORIO/blob/main/clonar_voz_xtts.ipynb)

> ⚠️ **Uso ético:** clona únicamente tu propia voz o la de personas que te hayan dado su permiso explícito.

---

## ✨ Qué hace

- Convierte y normaliza tu audio de entrenamiento.
- Lo transcribe y lo corta automáticamente en clips con **Whisper (large-v3)**.
- Descarta repeticiones y textos inventados, y convierte los números a letras.
- Permite revisar y corregir las transcripciones dentro del notebook, sin Excel.
- Prueba la clonación **zero-shot** (sin entrenar) y hace **fine-tuning** de XTTS-v2 con tu voz.
- Genera audio de **textos largos** dividiéndolos en frases y uniéndolas, con control de velocidad, pausas y volumen.

## 🧱 Tecnologías

| Componente | Herramienta |
|---|---|
| Modelo de voz | XTTS-v2 (`coqui-tts`) |
| Transcripción | `faster-whisper` (large-v3) |
| Procesamiento de audio | `pydub`, `soundfile` |
| Entorno | Google Colab (GPU T4) |

## 📋 Requisitos

- Cuenta de Google (para Colab y, opcionalmente, Drive).
- Un audio tuyo de **20 minutos o más**, limpio, sin eco ni ruido de fondo (WAV, MP3 o M4A).
- GPU T4 en Colab: *Entorno de ejecución → Cambiar tipo de entorno → GPU T4*.

## 🚀 Cómo usarlo

1. Abre `clonar_voz_xtts.ipynb` en Colab (botón de arriba o *Archivo → Subir notebook*).
2. Ejecuta las celdas **en orden**:

| Parte | Qué hace |
|---|---|
| 0 | Instala dependencias (después, **reinicia la sesión**) |
| 1 | Sube y prepara tu audio |
| 2 | Transcribe y corta en clips |
| 3 | Revisa y corrige las transcripciones |
| 4 | Descarga el modelo base y prueba zero-shot |
| 5 | Fine-tuning con tu voz |
| 6 | Carga tu modelo entrenado y guárdalo en Drive |
| 7 | Genera audio de textos largos |

3. Pega tu texto en `TEXTO_LARGO` (Parte 7), ejecuta y descarga `resultado.wav`.

### Revisar transcripciones

```python
escuchar("clip_0005")                      # oír el clip y ver su texto
corregir("clip_0005", "Texto correcto.")   # cambiar el texto
borrar("clip_0017")                        # eliminar un clip malo
verificar()                                # comprobar todo antes de entrenar
```

## 🎛️ Ajustes de calidad

| Problema | Qué ajustar |
|---|---|
| Voz muy lenta | `velocidad=1.15` (rango útil: 1.0 a 1.25) |
| Volumen bajo | `ganancia=2.0` a `3.0` |
| Suena monótona | `temperatura=0.75` |
| Inventa sílabas o se repite | `temperatura=0.5` a `0.6` |
| Palabra mal pronunciada | Escribirla como suena (`"fine tuning"` → `"fain tiuning"`) |
| Demasiadas pausas | `pausa_s=0.05` |

**Sobre la calidad:** el resultado depende sobre todo de las grabaciones. Un audio hablado de forma natural y con energía da mejor ritmo que uno leído de un artículo, y más audio (30 a 60 minutos) mejora la fidelidad.

## 💾 Qué guardar

Para generar audio sin volver a entrenar solo necesitas:

- `modelo_final.pth` (~1.8 GB)
- `referencia.wav`
- `config.json` y `vocab.json`

Los checkpoints de entrenamiento (`best_model_*.pth`, `checkpoint_*.pth`) pesan ~5 GB cada uno por incluir el optimizador y no son necesarios para generar voz.

## 📁 Estructura del repositorio

```
.
├── clonar_voz_xtts.ipynb   # notebook completo
├── README.md
├── .gitignore
└── LICENSE
```

> No subas tu audio, tu dataset ni los modelos entrenados: son datos biométricos tuyos y además pesan varios GB. El `.gitignore` incluido los excluye.

## 🛠️ Problemas frecuentes

| Error | Solución |
|---|---|
| `isin_mps_friendly` o `circular import` al importar `TTS` | Versiones incompatibles: usa el comando de instalación de la Parte 0 y reinicia la sesión |
| `open() got an unexpected keyword argument 'metadata_errors'` | Choque de `faster-whisper` con PyAV: el notebook ya evita este problema pasando el audio decodificado |
| `LJSpeech format expects 3 pipe-delimited columns` | Una línea de `metadata.csv` está mal. Ejecuta `verificar()`. No edites el archivo con Excel |
| `CUDA out of memory` | `BATCH=1` y `ACUM=4` en la Parte 5 |
| Pide `torchcodec` | `!pip install -q torchcodec` |
| El entrenamiento parece congelado | Guardar checkpoints es lento en Colab. Usa `save_step=100000` |

## ⚖️ Licencias

- El código de este repositorio se publica bajo la licencia indicada en `LICENSE`.
- **XTTS-v2 se distribuye bajo la [Coqui Public Model License](https://coqui.ai/cpml), de uso no comercial.** Si planeas un uso comercial, revisa alternativas como F5-TTS, Piper o Chatterbox y sus licencias.

## 🙌 Créditos

- [Coqui TTS / XTTS-v2](https://github.com/idiap/coqui-ai-TTS)
- [faster-whisper](https://github.com/SYSTRAN/faster-whisper) y [OpenAI Whisper](https://github.com/openai/whisper)

---

Hecho por **TU_NOMBRE** · [LinkedIn](https://linkedin.com/in/TU_PERFIL)
