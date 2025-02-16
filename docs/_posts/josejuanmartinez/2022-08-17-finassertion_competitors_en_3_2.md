---
layout: model
title: Identify Competing Organizations
author: John Snow Labs
name: finassertion_competitors
date: 2022-08-17
tags: [en, finance, assertion, status, competitors, licensed]
task: Assertion Status
language: en
edition: Spark NLP for Finance 1.0.0
spark_version: 3.2
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This models allows you to identify ORG and PRODUCTS mentioned in the text to be from a competitor. By default, if nothing is mentioned, it returns `NO_COMPETITOR`.

## Predicted Entities

`NO_COMPETITOR`, `COMPETITOR`

{:.btn-box}
[Live Demo](https://demo.johnsnowlabs.com/finance/ASSERTIONDL_COMPETITORS/){:.button.button-orange}
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/finance/models/finassertion_competitors_en_1.0.0_3.2_1660735220316.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
# Annotator that transforms a text column from dataframe into an Annotation ready for NLP

from sparknlp_jsl.annotator import *

documentAssembler = DocumentAssembler()\
    .setInputCol("text")\
    .setOutputCol("document")

# Sentence Detector annotator, processes various sentences per line
sentenceDetector = SentenceDetector()\
    .setInputCols(["document"])\
    .setOutputCol("sentence")

# Tokenizer splits words in a relevant format for NLP
tokenizer = Tokenizer()\
    .setInputCols(["sentence"])\
    .setOutputCol("token")

embeddings = BertEmbeddings.pretrained("bert_embeddings_sec_bert_base","en") \
    .setInputCols(["sentence", "token"]) \
    .setOutputCol("embeddings")

ner_model_org = FinanceNerModel().pretrained("finner_orgs_prods_alias", "en", "finance/models")\
    .setInputCols(["sentence", "token", "embeddings"])\
    .setOutputCol("ner")

ner_converter = NerConverterInternal() \
    .setInputCols(["sentence", "token", "ner"]) \
    .setOutputCol("ner_chunk")\
    .setWhiteList(['ORG', 'PRODUCT'])

assertion = AssertionDLModel.pretrained("finassertion_competitors", "en", "finance/models")\
    .setInputCols(["sentence", "ner_chunk", "embeddings"]) \
    .setOutputCol("assertion")
    
nlpPipeline = Pipeline(stages=[
    documentAssembler, 
    sentenceDetector,
    tokenizer,
    embeddings,
    ner_model_org,
    ner_converter,
    assertion
    ])

text = "Our competitors include the following by general category: legacy antivirus product providers, such as McAfee LLC and Broadcom Inc."

data = spark.createDataFrame([[text]]).toDF("text")

model = nlpPipeline.fit(data)

model.transform(spark.createDataFrame([[text]]).toDF("text")).select(F.explode(F.arrays_zip('ner_chunk.result', 'assertion.result')).alias('result')).show(truncate=False)
```

</div>

## Results

```bash
+--------------------------+
|result                    |
+--------------------------+
|[McAfee LLC, COMPETITOR]  |
|[Broadcom Inc, COMPETITOR]|
+--------------------------+
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|finassertion_competitors|
|Type:|finance|
|Compatibility:|Spark NLP for Finance 1.0.0+|
|License:|Licensed|
|Edition:|Official|
|Input Labels:|[document, doc_chunk, embeddings]|
|Output Labels:|[assertion]|
|Language:|en|
|Size:|2.2 MB|

## References

In-house annotations from 10K Filings

## Benchmarking

```bash
label	 tp	 fp	 fn	 prec	 rec	 f1
NO_COMPETITOR	 158	 0	 1	 1.0	 0.9937107	 0.9968454
COMPETITOR	 25	 1	 0	 0.96153843	 1.0	 0.98039216
tp: 183 fp: 1 fn: 1 labels: 2
Macro-average	 prec: 0.9807692, rec: 0.9968554, f1: 0.9887469
Micro-average	 prec: 0.9945652, rec: 0.9945652, f1: 0.9945652
```
