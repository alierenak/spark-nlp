---
layout: model
title: Context Spell Checker Pipeline for English
author: ahmedlone127
name: spellcheck_dl_pipeline
date: 2022-06-14
tags: [spellcheck, spell, spellcheck_pipeline, spelling_corrector, en, open_source]
task: Spell Check
language: en
edition: Spark NLP 4.0.0
spark_version: 3.0
supported: false
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This pretrained spellchecker pipeline is built on the top of [spellcheck_dl](https://nlp.johnsnowlabs.com/2022/04/02/spellcheck_dl_en_2_4.html) model. This pipeline is for PySpark 2.4.x users with SparkNLP 3.4.2 and above.

## Predicted Entities



{:.btn-box}
[Live Demo](https://demo.johnsnowlabs.com/healthcare/CONTEXTUAL_SPELL_CHECKER/){:.button.button-orange}
[Open in Colab](https://colab.research.google.com/github/JohnSnowLabs/spark-nlp-workshop/blob/master/tutorials/streamlit_notebooks/healthcare/CONTEXTUAL_SPELL_CHECKER.ipynb){:.button.button-orange.button-orange-trans.co.button-icon}
[Download](https://s3.amazonaws.com/community.johnsnowlabs.com/ahmedlone127/spellcheck_dl_pipeline_en_4.0.0_3.0_1655213660333.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python

pipeline = PretrainedPipeline("spellcheck_dl_pipeline", lang = "en")

text = ["During the summer we have the best ueather.", "I have a black ueather jacket, so nice."]

pipeline.annotate(text)
```
```scala

val pipeline = new PretrainedPipeline("spellcheck_dl_pipeline", lang = "en")

val example = Array("During the summer we have the best ueather.", "I have a black ueather jacket, so nice.")

pipeline.annotate(example)
```
</div>

## Results

```bash

[{'checked': ['During', 'the', 'summer', 'we', 'have', 'the', 'best', 'weather', '.'],
  'document': ['During the summer we have the best ueather.'],
  'token': ['During', 'the', 'summer', 'we', 'have', 'the', 'best', 'ueather', '.']},

 {'checked': ['I', 'have', 'a', 'black', 'leather', 'jacket', ',', 'so', 'nice',  '.'],
  'document': ['I have a black ueather jacket, so nice.'],
  'token': ['I', 'have', 'a', 'black', 'ueather', 'jacket', ',', 'so', 'nice', '.']}]
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|spellcheck_dl_pipeline|
|Type:|pipeline|
|Compatibility:|Spark NLP 4.0.0+|
|License:|Open Source|
|Edition:|Community|
|Language:|en|
|Size:|99.8 MB|

## Included Models

- DocumentAssembler
- TokenizerModel
- ContextSpellCheckerModel