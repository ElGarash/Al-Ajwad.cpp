# Al Ajwad with whisper.cpp

Converting the model to ggml format and running the model.

# Prerequisites

1. Open the devcontainer for easy setup (Optional).
2. Enable git-lfs `git lfs install`
3. Download the model `git clone https://huggingface.co/omartariq612/whisper-small-everyayah model` this will take some time depending on the model size.

4. Build the whisper.cpp with GPU support.
```bash
cd whisper.cpp
# you need to have nvidia-docker installed on your host, and update CUDA_DOCKER_ARCH to match your GPU architecture
GGML_CUDA=1 CUDA_DOCKER_ARCH=sm_75 make -j 
```

5. you may need to update the model config (see update_config.patch)

6. Convert the model to ggml format
```bash
poetry install
python ./whisper.cpp/models/convert-h5-to-ggml.py ./model ./whisper out
```

6. Run the model
```bash
./whisper.cpp/main -m out/ggml-model.bin  -bs 8 -bo 8 -l ar -of t -otxt -nt example.wav
```
