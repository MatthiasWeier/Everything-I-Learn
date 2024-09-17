# Prerequisites and Setup

1. **Python Environment**
   - Ensure you have **Python 3.8** or higher installed. You can check your Python version with:
     ```bash
     python --version
     ```

2. **Install Required Python Libraries**
   You need to install several libraries for the script to function properly. Follow these steps to install them.

   #### Required Libraries:
   - **Torch (PyTorch)**: Required for GPU/CPU acceleration and model handling.
   - **Bark**: The core text-to-speech library, which can be installed directly from GitHub.
   - **nltk**: Used for sentence tokenization.
   - **numpy**: For handling arrays.
   - **scipy**: Specifically `scipy.io.wavfile` is used to write the WAV file.
   - **pydub**: For audio segment handling and concatenation.
   - **ffmpeg** (external): `pydub` requires `ffmpeg` or `libav` for working with audio files.

   #### Install the Required Libraries:

   First, install the Bark library directly from GitHub:
   ```bash
   pip install git+https://github.com/suno-ai/bark.git
   ```

   Next, install the other dependencies:
   ```bash
   pip install torch nltk numpy scipy pydub
   ```

   #### Install FFmpeg:
   `pydub` requires FFmpeg for audio handling. Install FFmpeg depending on your operating system:
   - **Windows**: Download from [ffmpeg.org](https://ffmpeg.org/download.html) and add to your system's PATH.
   - **macOS**: Install via Homebrew:
     ```bash
     brew install ffmpeg
     ```
   - **Linux**: Install via package manager:
     ```bash
     sudo apt-get install ffmpeg
     ```

3. **Download NLTK Data**
   The script uses NLTK’s `sent_tokenize` function, which requires downloading the sentence tokenization data. The script automatically attempts to download the required data via:
   ```python
   nltk.download('punkt')
   ```

   You can also run this manually:
   ```bash
   python -m nltk.downloader punkt
   ```

4. **Ensure GPU (Optional)**
   If you want to leverage GPU for faster TTS processing, ensure that your system has a compatible GPU (NVIDIA CUDA-enabled GPU) and the appropriate **CUDA drivers** and **cuDNN** installed. If you have a NVIDIA GPU I highly suggest using CUDA, this will boost the speed by a lot. For AMD GPUs this script does not provide GPU usage. They will need to rely on CPU only.

   - Verify GPU support with:
     ```bash
     python -c "import torch; print(torch.cuda.is_available())"
     ```
   - If `torch.cuda.is_available()` returns `False`, ensure you have the correct version of CUDA installed.

5. **Rename the Script**
   Rename the script file from `BarkServer.py` to `BarkScript.py`.

   Example command to rename the file:
   ```bash
   mv BarkServer.py BarkScript.py
   ```

6. **Running the Script**
   Once everything is set up, you can run the script from the command line with the new name:

   ```bash
   python BarkScript.py "Hello, this is a test of the Bark text-to-speech system." output.wav
   ```

7. **Optional**: GPU Usage
   - Ensure you have CUDA drivers installed and PyTorch with CUDA support if you want to speed up the process by using a GPU.

### Summary of Steps:

1. **Python 3.8+**
2. Install the Bark library:
   ```bash
   pip install git+https://github.com/suno-ai/bark.git
   ```
3. Install the other required libraries:
   ```bash
   pip install torch nltk numpy scipy pydub
   ```
4. Install FFmpeg for `pydub`.
5. Download NLTK data (`punkt`):
   ```bash
   python -m nltk.downloader punkt
   ```
6. Rename the script:
   ```bash
   mv BarkServer.py BarkScript.py
   ```
7. Run the script with the new name:
   ```bash
   python BarkScript.py "Your text here" output.wav
   ```

Once everything is installed and the setup is complete, you should be able to run the script successfully to generate TTS audio using Bark.

``` python
import sys
import torch
from bark import SAMPLE_RATE, generate_audio, preload_models
from scipy.io.wavfile import write as write_wav
import nltk
import numpy as np
from pydub import AudioSegment

# Download NLTK data
nltk.download('punkt')

# Ensure GPU usage
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

# Preload models
preload_models()

# Specify the consistent voice using history_prompt
history_prompt = "v2/en_speaker_2"  # replace with the desired speaker

# Function to split text into sentences
def split_text(text):
    from nltk.tokenize import sent_tokenize
    sentences = sent_tokenize(text)
    return sentences

# Function to combine audio segments
def combine_audio(audio_segments):
    combined = AudioSegment.empty()
    for segment in audio_segments:
        combined += segment
    return combined

def main():
    if len(sys.argv) < 3:
        print("Usage: BarkServer.py <text> <output_wav_file>")
        print(f"Received arguments: {sys.argv}")
        return

    # Join all arguments except the last one to reconstruct the text
    text = " ".join(sys.argv[1:-1])
    output_wav_file = sys.argv[-1]

    try:
        print(f"Processing text: {text}")
        print(f"Output file will be: {output_wav_file}")

        # Split text into sentences
        sentences = split_text(text)
        print(f"Split text into {len(sentences)} sentences.")

        # Generate TTS for each sentence and combine audio
        audio_data = []

        for sentence in sentences:
            audio_array = generate_audio(sentence, history_prompt=history_prompt)  # Use the consistent voice
            audio_data.append(audio_array)

        concatenated_audio = np.concatenate(audio_data)

        # Save the combined audio to a WAV file
        write_wav(output_wav_file, SAMPLE_RATE, concatenated_audio.astype(np.float32))

        print(f"Audio saved to {output_wav_file}")

    except Exception as e:
        print(f"Error generating TTS: {e}")

if __name__ == "__main__":
    main()

```

## Example audio generation
```bash
python C:\Path\To\BarkServer.py "Hello, fellow repository explorer! I hope you enjoy this little demo and consider trying out Bark yourself. If you appreciate Bark, don’t forget to give it a star on GitHub! And if you like what you find in my repository, feel free to leave a star here as well." DemoOutput.wav
```
![Demo Output Wav](DemoOutput.webm)
## More then this
- I only needed this part of Bark, thats why not more is in the script
- If you use no voice --> bark generates a random voice --> you can save that random voice as a npz file. (Maybe it's cool?)
- Apparently Bark can use AMD GPU. I wasn't able to make it work. If anyone can do it please write me I need it.
- Bark can be used in several languages.
- You can also implement emotions. Example [laughter], [sighs], even [music] for more details go to their original repository under "Details"

# Good to knows

- Why am i splitting sentences? -->  Bark is a GPT-style model, and its architecture/context window is optimized to output generations with roughly 14 seconds.
- You can change the voices example: history_prompt = "v2/en_speaker_2"  --> "v2/en_speaker_1"
- [Here is a list of preloaded voices to use](https://github.com/rsxdalv/tts-generation-webui/blob/main/tts_webui/bark/get_speaker_gender.py)
- [Original Repository](https://github.com/suno-ai/bark)
- [Bark Infinity - JonathanFly](https://github.com/JonathanFly/bark?tab=readme-ov-file
- [Bark tts-generation-webui](https://github.com/rsxdalv/tts-generation-webui/tree/main)

## Example scenarios to use this script for

- **Audiobook Creation**: Whether it's for a professional project or just a bedtime story for your kids, this script can help you turn any text into a personalized audiobook. (I haven't tried Bark Infinity, but it's probably well-suited for this.)
- **School Projects**: I once needed to add voiceovers to each slide in a PowerPoint presentation. If Bark had been available then, I would have definitely used it to make that process easier.
- **Language Learning Tools**: My parents are learning English, and this tool could help them by demonstrating how to pronounce sentences with proper emphasis and clarity.
- **YouTube Narration**: I use Bark TTS to provide voiceovers for YouTube videos generated by an automated tool. It’s a great fit for quick, high-quality narration.
- **Educational Uses (Teachers and Professors)**: Why spend hours recording lectures when you can have Bark deliver them for you? It’s a time-saver for educators.
- **Self-Learning**: Instead of paying for expensive courses, you can use AI to create a script and have it read out loud. Many lessons are already available online – this tool just brings them to life in audio form.
- **Smart Home Integration**: Imagine creating your own Alexa-like assistant, but with a better, more natural voice and fully running locally for privacy. While this requires a decent PC for now, future hardware advancements will make real-time responses even faster and more accessible.
- **Museum Audio Descriptions**: Traditionally, museums hire narrators for audio tours. Now, you can use Bark to generate realistic, human-like voices for exhibit descriptions on demand.
- If you want to create songs: [suno AI](https://suno.com) These are the same that worked on Bark. I talked to a developer of them and apparently they use Bark like models / scripts to make this music. With suno AI you can create fully songs (roughly 3-4 minutes). I highly suggest you to try it. It probably will be better then you expect.