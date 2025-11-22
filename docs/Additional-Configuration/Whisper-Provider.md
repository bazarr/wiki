# Whisper Provider Setup

Note: Whisper is capable of **transcribing** many languages, but can only **translate** a language into English. It does not support translating to other languages.

Whisper (based on [OpenAI Whisper](https://github.com/openai/whisper)) uses a neural network powered by your CPU or NVIDIA graphics card to generate subtitles for your media.

Whisper supports transcribing in many languages as well as translating from a language to English. The provider works best when it knows the audio language ahead of time. Make sure the 'Deep analyze media file to get audio tracks language' option is enabled to ensure the best results.

Minimum score must be lowered if you want whisper generated subtitles to be automatically "downloaded" because they have a fixed score which is 241/360 (~67%) for episodes and 61/120 (~51%) for movies.

## whisper-asr-webservice [SubGen]

Bazarr's Whisper provider communicates with [SubGen](https://github.com/McCloudS/subgen?tab=readme-ov-file#docker). Refer to its [documentation](https://github.com/McCloudS/subgen/blob/main/README.md) for more assistance in setting it up.

## Choosing a Model

Larger models are more accurate but take longer to run. Choose the largest model you are comfortable with and your CPU/GPU is capable of running.

Available ASR_MODELs are `tiny`, `base`, `small`, `medium`, `large` (only OpenAI Whisper), `large-v1`, `large-v2` and `large-v3` (only OpenAI Whisper for now).

## Choosing a backend

whisper-asr-webservice supports multiple backends. Currently, there are two available options:

* [openai_whisper](https://github.com/openai/whisper) (original implementation)
* [faster_whisper](https://github.com/SYSTRAN/faster-whisper)

## Docker Installation

The complete Docker Compose file can be found [here](https://github.com/McCloudS/subgen/blob/main/docker-compose.yml).

### Prerequsites for the below examples

#### cache dir for the models

```bash
mkdir -p {subgenai_data_folder}/models
```

#### .env file

```yaml
subgenai_data_folder=/path/to/your/subgen
```

#### subgen.env file

```yaml
WHISPER_MODEL=medium
WHISPER_THREADS=4
WEBHOOKPORT=9000
TRANSCRIBE_DEVICE=cuda
DEBUG=True
CLEAR_VRAM_ON_COMPLETE=True
APPEND=False
TRANSCRIBE_OR_TRANSLATE=transcribe
NAMESUBLANG=ai
```

### GPU (recommended)

```yaml
--8<-- "includes/Additional-Configuration/whisper-gpu.yml"
```

### CPU

```yaml
--8<-- "includes/Additional-Configuration/whisper-cpu.yml"
```

## Docker on Windows

You can run the ASR container on your Windows PC. This is especially useful if its your only way to get access to a powerful GPU. Follow [these instructions](https://docs.docker.com/desktop/wsl/) for more information.

## Bazarr Configuration

Change the endpoint to the server you are hosting the Whisper container on (127.0.0.1 if on the same machine), and adjust the timeout if you find it keeps timing out on long movies or TV shows. **The endpoint must start with http://**

![img.png](images/whisper_config.png)

## Troubleshooting

### Language detection

When Bazarr doesn't know the language of the media you're trying to get subtitles for, Whisper must guess. It only uses the first 30 seconds of audio in order to detect the language. To ensure best results, use media which has the audio language specified in the file, and make sure the deep analyze option is turned on.

Bazarr's implementation of Whisper is still in early stages. If you have any issues, follow these instructions:

* Enable debug logging
* Join the Bazarr discord and post your logs, and a description of the problem on the #whisper channel
* Include an @Alex mention to get my attention
