# Al Ajwad with whisper.cpp

Running the model using whisper.cpp.

# Prerequisites

1. Open the devcontainer for easy setup (Optional).
2. Enable git-lfs `git lfs install`
3. Download the model using [HuggingFaceModelDownloader](https://github.com/bodaay/HuggingFaceModelDownloader) this will take some time depending on the model size.

```bash
bash <(curl -sSL https://g.bodaay.io/hfd) -m omartariq612/whisper-small-everyayah 
```

For Legacy models (without safetensors) convert to safetensor using the 🤗 [space](https://huggingface.co/spaces/safetensors/convert) if it's not already converted by the bot. Then download the model using [HuggingFaceModelDownloader](https://github.com/bodaay/HuggingFaceModelDownloader): `bash <(curl -sSL https://g.bodaay.io/hfd) -m tarteel-ai/whisper-tiny-ar-quran -b "pr%2F1"` here we are downloading from the branch named `pr/1` (the branch created by the bot).

4. Build the whisper.cpp with GPU support.
To get the `CUDA_DOCKER_ARCH` env var value, on the host machine run

```bash
$ nvidia-container-cli info
NVRM version:   550.90.07
CUDA version:   12.4

Device Index:   0
Device Minor:   0
Model:          NVIDIA GeForce GTX 1650
Brand:          GeForce
GPU UUID:       GPU-24b85241-751e-fd5f-2193-039a04c82c9a
Bus Location:   00000000:01:00.0
Architecture:   7.5
```
`CUDA_DOCKER_ARCH=sm_{architecture_without_the_dot}`
  
```bash
cd whisper.cpp
# to get 
# you need to have nvidia-docker installed on your host, and update CUDA_DOCKER_ARCH to match your GPU architecture
GGML_CUDA=1 CUDA_DOCKER_ARCH=sm_75 make -j 
```

5. you may need to update the model config (see update_config.patch)

6. Convert the model to ggml format
```bash
mkdir -p out/alajwad
poetry run python ./whisper.cpp/models/convert-h5-to-ggml.py ./model ./whisper out/alajwad
```

# Happy transcription 🎉
6. Run the model
```bash
./whisper.cpp/main -m out/ggml-model.bin  -bs 8 -bo 8 -l ar -of t -otxt -nt example.wav
```
