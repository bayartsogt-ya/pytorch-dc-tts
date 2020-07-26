PyTorch implementation of
[Efficiently Trainable Text-to-Speech System Based on Deep Convolutional Networks with Guided Attention](https://arxiv.org/abs/1710.08969)
based partially on the following projects:
* https://github.com/Kyubyong/dc_tts (audio pre processing)
* https://github.com/r9y9/deepvoice3_pytorch (data loader sampler)

## Online Text-To-Speech Demo
The following notebooks are executable on [https://colab.research.google.com ](https://colab.research.google.com):
* [Mongolian Male Voice TTS Demo](https://colab.research.google.com/github/tugstugi/pytorch-dc-tts/blob/master/notebooks/MongolianTTS.ipynb)
* [English Female Voice TTS Demo (LJ-Speech)](https://colab.research.google.com/github/tugstugi/pytorch-dc-tts/blob/master/notebooks/EnglishTTS.ipynb)

For audio samples and pretrained models, visit the above notebook links.

## Training/Synthesizing English Text-To-Speech
The English TTS uses the [LJ-Speech](https://keithito.com/LJ-Speech-Dataset/) dataset.
1. Download the dataset: `python dl_and_preprop_dataset.py --dataset=ljspeech`
2. Train the Text2Mel model: `python train-text2mel.py --dataset=ljspeech`
3. Train the SSRN model: `python train-ssrn.py --dataset=ljspeech`
4. Synthesize sentences: `python synthesize.py --dataset=ljspeech`
   * The WAV files are saved in the `samples` folder.

## Training/Synthesizing Mongolian Text-To-Speech
The Mongolian text-to-speech uses 5 hours audio from the [Mongolian Bible](https://www.bible.com/mn/versions/1590-2013-ariun-bibli-2013).
1. Download the dataset: `python dl_and_preprop_dataset.py --dataset=mbspeech`
2. Train the Text2Mel model: `python train-text2mel.py --dataset=mbspeech`
3. Train the SSRN model: `python train-ssrn.py --dataset=mbspeech`
4. Synthesize sentences: `python synthesize.py --dataset=mbspeech`
   * The WAV files are saved in the `samples` folder.

## FINETUNE Mongolian Text-To-Speech on your CUSTOM dataset
Prepare audio files as stated below and 
1. First, prepare your dataset like below:
```
| -- datasets/
| -- | -- MBSpeech-1.0/
| -- | -- | -- wavs/
| -- | -- | -- | -- filename.wav
| -- | -- | -- | -- ...
| -- | -- | -- metadata.csv
```
In this case your metadata.csv looks like this (`|` separated file):
```
filename|ямар нэг текст|ямар нэг текст
```
Now prepare the dataset: `python dl_and_preprop_dataset.py --dataset=custom_dataset`
2. Download pretrained model: 
```
mkdir logdir/

mkdir logdir/mbspeech-text2mel/
wget -q -O logdir/mbspeech-text2mel/mbspeech-text2mel.pth https://www.dropbox.com/s/wu26k6tu5hz8hq1/step-200K.pth

mkdir logdir/mbspeech-ssrn/
wget -q -O logdir/mbspeech-ssrn/mbspeech-ssrn.pth https://www.dropbox.com/s/tel0xcqa7kkwqze/step-165K.pth
```
3. Train the Text2Mel model: `python train-text2mel.py --dataset=mbspeech`
4. Train the SSRN model: `python train-ssrn.py --dataset=mbspeech`
5. Synthesize sentences: `python synthesize.py --dataset=mbspeech`
   * The WAV files are saved in the `samples` folder.