# Korean TTS Model: what is the best Hangul processing strategy for Korean speech synthesis?

한글은 주로 한국어를 위해 고안된 독창적 인 스크립트입니다. 
그것은 라틴 문자와 같은 원칙적으로 발음이지만, 독일어 또는 스페인어보다 정확하게 발음 규칙을 알아야 더 발음 규칙을 알 필요가 있습니다. 
한글은 한자와 같은 음절이며, 한글은 가나와 다른 점은 한글 음절이 헌법 자음과 모음으로 분해 될 수 있다는 점입니다. 
한데 모아서, 이것은 실제의 가독성을 위해 매우 편리하지만 종종 한국의 컴퓨터 언어학자를 난처하게합니다. 
먼저 그래프를 음소로 변환해야합니까? 한글 음절을 TTS 용으로 분해하는 것이 더 낫습니까? 아니면 분해없이 음절을 가져 가야합니까? 
한글 유니 코드 뒤에 숨어있는 장면을 안다면, 상황은 더욱 복잡해 질 것입니다.
한글 자모 (0x01100-0x011FF)와 한글 호환성 자모 (0x03130-0x0318F)는 현대 한글 자음과 모음 (한국어로 jamo이라고 함)을위한 두 가지 종류의 유니 코드 블록이 있습니다. 
한글과의 호환성 Jamo는 첫 자음 (시작)과 최종 자음 (코드)에 동일한 유니 코드 포인트가 주어지며 한글 자모에서는 독립적 인 문자로 취급됩니다. 
(비 유적으로, 영어로 한글 자모 체계를 따라한다면 법에 따라 두 가지를 구별해야합니다.) 한편, 두 사람은 ㄲ, ㄱㅅ 같은 자음 클러스터를 하나의 편지로 간주합니다. 
어떤 이들은 하나의 자음 시퀀스로 이해해야한다고 주장합니다. 컴퓨터 실습에있어 맞습니까? 이 질문들은이 프로젝트에 동기를 부여합니다.

I run four different experiements depending on the Hangul processing strategies below.

* Exp.0: Hangul Jamo (0x01100-0x011FF) with consonant clusters. Graphemes are converted into phonemes.
* Exp.1: Hangul Jamo (0x01100-0x011FF) with consonant clusters.
* Exp.2: Hangul Compatibility Jamo (0x03130-0x0318F) with consonant clusters
* Exp.3: Hangul Jamo (0x01100-0x011FF). Single consonants only.
* Exp.4: Hangul Compatibility Jamo (0x03130-0x0318F). Single consonants only.

## Requirements
  * python >= 2.7
  * NumPy >= 1.11.1
  * TensorFlow >= 1.3
  * librosa
  * tqdm
  * matplotlib
  * scipy

## Data

[KSS Dataset](https://www.kaggle.com/bryanpark/korean-single-speaker-speech-dataset/version/2), a Korean single speaker speech dataset, is used.

## Model
DCTTS, introudced in [Efficiently Trainable Text-to-Speech System Based on Deep Convolutional Networks with Guided Attention](https://arxiv.org/abs/1710.08969), is implemented for this project.
You can refer to my other repo to see the original implementation. This repo focuses on the comparison among the four different experiment conditions.

## Training
  * STEP 0. Download [KSS Dataset](https://www.kaggle.com/bryanpark/korean-single-speaker-speech-dataset).
  * STEP 1. Adjust `num_exp` in `hyperparams.py`.
  * STEP 2. Run `python prepro.py` for model inputs and targets.
  * STEP 3. Run `python train.py 1` for training Text2Mel.
  * STEP 4. Run `python train.py 2` for training SSRN.

You can do STEP 3 and 4 at the same time, if you have more than one gpu card.


## Sample Synthesis
  * Run `synthesize.py` and check the files in `samples`.

## Generated Samples

| Num Experiment       | Samples |
| :----- |:-------------|
| 0      | [400k](https://soundcloud.com/kyubyong-park/sets/kss_exp0)|
| 1      | [400k](https://soundcloud.com/kyubyong-park/sets/kss_exp1)|
| 2      | [400k](https://soundcloud.com/kyubyong-park/sets/kss_exp2)|
| 3| [400k](https://soundcloud.com/kyubyong-park/sets/kss_ex3)|
|4 | [400k](https://soundcloud.com/kyubyong-park/sets/kss_exp4)|

## Pretrained Models

| Num Experiment       | Models |
| :----- |:-------------|
| 0      | [400k](https://www.dropbox.com/s/ipt17hoo4lj56xg/exp0.zip?dl=0)|
| 1      | [400k](https://www.dropbox.com/s/q133hrwyyvudl65/exp1.zip?dl=0)|
| 2      | [400k](https://www.dropbox.com/s/vaz0tb5l8gwfvd0/exp2.zip?dl=0)|
| 3| [400k](https://www.dropbox.com/s/iy7v2zzqguw1q18/exp3.zip?dl=0)|
|4 | [400k](https://www.dropbox.com/s/qtxiss3jk0hjbap/exp4.zip?dl=0)|

## Notes

  * Refer to [this](https://github.com/Kyubyong/kss/blob/master/graph2pron_statistics.md), which is provided by Hyungjun So.
