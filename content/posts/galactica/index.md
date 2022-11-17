---
title: "GALACTICAをローカルで動かす"
description: "GALACTICAをローカルで動かす"
date: 2022-11-16T12:00:00+09:00
summary: "MetaAIの研究チームが公開したGalacticaをローカルで動かしたメモ"
draft: false
author: "kame404"
tags: [python,AI]
---

`MetaAI`の研究チームが公開した[Galactica](https://galactica.org)をローカルで動かします。
`Galactica`は大量の研究論文や資料で訓練されているそうです。

|Paper|GitHubリポジトリ|
|-|-|
|[Galactica: A Large Language Model for Science](https://galactica.org/static/paper.pdf)|[galai](https://github.com/paperswithcode/galai)|

{{< tweet user="paperswithcode" id="1592546933679476736" >}}



## 実行環境
+ Windows 11 Version 22H2 (OS Build 25236.1010)
+ Miniconda3 Windows 64-bit
+ NVIDIA GeForce GTX 1070 8GB

## 仮想環境作成
`Miniconda3`のプロンプトを開き、`python3.9`の仮想環境を作ります

```python
conda create -n galactica python=3.9
```
仮想環境を`activate`します
```python
conda activate galactica
```
`pip`でインストールします
```python
pip install git+https://github.com/paperswithcode/galai
```

## 実行してみる
`Lecture 1: The Ising Model`と入力して、イジング模型について書いてもらいましょう

```python
import galai as gal

model = gal.load_model("base")
model.generate("Lecture 1: The Ising Model\n\n", new_doc=True, top_p=0.7, max_length=200)
```

> AssertionError: Torch not compiled with CUDA enabled

`AssertionError`が出てしまいました。PyTorch.orgの[START LOCALLYの手順](https://pytorch.org/get-started/locally/)に従って`CUDA`をセットアップします

```python
conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia
```

### 再挑戦
```python
import galai as gal

model = gal.load_model("base")
model.generate("Lecture 1: The Ising Model\n\n", new_doc=True, top_p=0.7, max_length=200)
```
> RuntimeError: CUDA error: invalid device ordinal

`RuntimeError`が出てしまいました。
GPUの数がデフォルト(8個)より少ない場合は、モデルをロードするときにGPUの数を指定する必要があるようです

### 再々挑戦
```python
import galai as gal

model = gal.load_model(name = 'base', num_gpus = 1)
model.generate("Lecture 1: The Ising Model\n\n", new_doc=True, top_p=0.7, max_length=200)
```

{{< katex >}}

> The Ising model is a prototypical example of a system that can be exactly solved, and this is why it is also a prototypical example of a critical system. The Ising model is also a prototypical example of a system that is solvable exactly in the infinite volume limit, and this is why it is also a prototypical example of a critical system. It is a simplified version of the Heisenberg model, and its hamiltonian can be expressed by the following formula.
\\(\mathcal{H} = -\sum_{\langle i,j\rangle} J \sigma_i\sigma_j\\)

数式を含む文章が生成されました。文中の\\(\mathcal{H} = -\sum_{\langle i,j\rangle} J \sigma_i\sigma_j\\)は実際にある数式のようです
[learn.microsoft.com Ising model](https://learn.microsoft.com/en-us/azure/quantum/optimization-concepts-ising-model-for-optimization)

```python
model = gal.load_model("base")
```
今回は`base`モデルを指定していますが、以下の5つのモデルを指定できます

|  Size       | Parameters  |
|:-----------:|:-----------:|
| `mini`      |    125 M    |
| `base`      |    1.3 B    |
| `standard`  |    6.7 B    |
| `large`     |     30 B    |
| `huge`      |    120 B    |