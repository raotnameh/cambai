### Problem Statement:
- To set up a streaming speaker diarization pipeline. The output folder contains the report on the task.

---

### Working:
1. The pipeline takes an audio file as input and outputs an RTTM file containing the speaker diarization results.
2. The pipeline is built using diart [1], which uses pyannote-Audio [2] as a base library for streaming speaker diarization.
3. It uses Dscore [3] to calculate the Diarization Error Rate (DER).

---

### Directory Structure:

The project directory contains the following subdirectories:

- **data**: Contains the data files.
- **src**: Contains the source code files.
  - `data.ipynb`: Contains the code used for downloading and creating the data.
  - `diar.ipynb`: Contains the code used in the offline speaker diarization pipeline using pyannote-Audio.
- **output**: Contains the output files.
- **utils**: Contains the utility files.

---

### Installation:
This project is tested on Linux. To install the project, follow the steps below:

1. Clone the repository.
2. Build the conda environment using the `environment.yml` file:

```bash
conda env create -f environment.yml
conda activate diart
```

---

### Usage

You need to provide a token or be logged in to Hugging Face using `huggingface-cli login` to download the models.
```bash
huggingface-cli login
```

Run the following command to start the speaker diarization pipeline:
```bash
# Use "microphone:ID" to select a non-default device
# See `python -m sounddevice` for available devices
diart.stream microphone
```
For more features or details, refer to the diart documentation [1]. Briefly, the available arguments are:
```bash
SEGMENTATION = "Segmentation model name from pyannote"
EMBEDDING = "Embedding model name from pyannote"
DURATION = "Chunk duration (in seconds)"
STEP = "Sliding window step (in seconds)"
LATENCY = "System latency (in seconds). STEP <= LATENCY <= CHUNK_DURATION"
TAU = "Probability threshold to consider a speaker as active. 0 <= TAU <= 1"
RHO = "Speech ratio threshold to decide if centroids are updated with a given speaker. 0 <= RHO <= 1"
DELTA = "Embedding-to-centroid distance threshold to flag a speaker as known or new. 0 <= DELTA <= 2"
GAMMA = "Parameter gamma for overlapped speech penalty"
BETA = "Parameter beta for overlapped speech penalty"
MAX_SPEAKERS = "Maximum number of speakers"
CPU = "Force models to run on CPU"
BATCH_SIZE = "For segmentation and embedding pre-calculation. If BATCH_SIZE < 2, run fully online and estimate real-time latency"
NUM_WORKERS = "Number of parallel workers"
OUTPUT = "Directory to store the system's output in RTTM format"
HF_TOKEN = "Hugging Face authentication token for hosted models ('true' | 'false' | <token>). If 'true', it will use the token from huggingface-cli login"
SAMPLE_RATE = "Sample rate of the audio stream"
NORMALIZE_EMBEDDING_WEIGHTS = "Rescale embedding weights (min-max normalization) to be in the range [0, 1]. This is useful in some models without weighted statistics pooling that rely on masking, like Nvidia's NeMo or ECAPA-TDNN"
```

---
### References: 
1. https://github.com/juanmc2005/diart/tree/main?tab=readme-ov-file
2. https://github.com/pyannote/pyannote-audio?tab=readme-ov-file
3. https://github.com/nryant/dscore