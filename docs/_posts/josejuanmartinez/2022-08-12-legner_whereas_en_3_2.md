---
layout: model
title: Legal NER - Whereas Clauses
author: John Snow Labs
name: legner_whereas
date: 2022-08-12
tags: [en, legal, ner, whereas, licensed]
task: Named Entity Recognition
language: en
edition: Spark NLP for Legal 1.0.0
spark_version: 3.2
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This is a Legal NER Model, able to process WHEREAS clauses, to detect the SUBJECT (Who?), the ACTION, the OBJECT (what?) and, in some cases, the INDIRECT OBJECT (to whom?) of the clause.

## Predicted Entities

`PARTY`, `EFFDATE`, `DOC`, `ALIAS`

{:.btn-box}
[Live Demo](https://demo.johnsnowlabs.com/finance/LEGALNER_WHEREAS/){:.button.button-orange}
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/legal/models/legner_whereas_en_1.0.0_3.2_1660294083004.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
documentAssembler = DocumentAssembler()\
        .setInputCol("text")\
        .setOutputCol("document")
        
sentenceDetector = SentenceDetectorDLModel.pretrained("sentence_detector_dl","xx")\
        .setInputCols(["document"])\
        .setOutputCol("sentence")

tokenizer = Tokenizer()\
        .setInputCols(["sentence"])\
        .setOutputCol("token")

embeddings = RoBertaEmbeddings.pretrained("roberta_embeddings_legal_roberta_base","en") \
    .setInputCols(["sentence", "token"]) \
    .setOutputCol("embeddings")

ner_model = LegalNerModel().pretrained('legner_whereas', 'en', 'legal/models')\
        .setInputCols(["sentence", "token", "embeddings"])\
        .setOutputCol("ner")

ner_converter = NerConverter()\
        .setInputCols(["sentence","token","ner"])\
        .setOutputCol("ner_chunk")

nlpPipeline = Pipeline(stages=[
        documentAssembler,
        sentenceDetector,
        tokenizer,
        embeddings,
        ner_model,
        ner_converter])

empty_data = spark.createDataFrame([[""]]).toDF("text")

model = nlpPipeline.fit(empty_data)

text = ["""WHEREAS, Seller and Buyer have entered into that certain Stock Purchase Agreement, dated November 14, 2018 (the "Stock Purchase Agreement"); WHEREAS, pursuant to the Stock Purchase Agreement, Seller has agreed to sell and transfer, and Buyer has agreed to purchase and acquire, all of Seller's right, title and interest in and to Armstrong Wood Products, Inc., a Delaware corporation ("AWP") and its Subsidiaries, the Company and HomerWood Hardwood Flooring Company, a Delaware corporation ("HHFC," and together with the Company, the "Company Subsidiaries" and together with AWP, the "Company Entities" and each a "Company Entity") by way of a purchase by Buyer and sale by Seller of the Shares, all upon the terms and condition set forth therein;"""]

res = model.transform(spark.createDataFrame([text]).toDF("text"))
```

</div>

## Results

```bash
+------------+-----------------+
|       token|        ner_label|
+------------+-----------------+
|     WHEREAS|                O|
|           ,|                O|
|      Seller|B-WHEREAS_SUBJECT|
|         and|                O|
|       Buyer|B-WHEREAS_SUBJECT|
|        have| B-WHEREAS_ACTION|
|     entered| I-WHEREAS_ACTION|
|        into| I-WHEREAS_ACTION|
|        that| B-WHEREAS_OBJECT|
|     certain| I-WHEREAS_OBJECT|
|       Stock| I-WHEREAS_OBJECT|
|    Purchase| I-WHEREAS_OBJECT|
|   Agreement| I-WHEREAS_OBJECT|
|           ,|                O|
|       dated|                O|
|    November|                O|
|          14|                O|
|           ,|                O|
|        2018|                O|
|           (|                O|
|         the|                O|
|           "|                O|
|       Stock|                O|
|    Purchase|                O|
|   Agreement|                O|
|         ");|                O|
|     WHEREAS|                O|
|           ,|                O|
|    pursuant|                O|
|          to|                O|
|         the|                O|
|       Stock|                O|
|    Purchase|                O|
|   Agreement|                O|
|           ,|                O|
|      Seller|B-WHEREAS_SUBJECT|
|         has| B-WHEREAS_ACTION|
|      agreed| I-WHEREAS_ACTION|
|          to| I-WHEREAS_ACTION|
|        sell| I-WHEREAS_ACTION|
|         and|                O|
|    transfer|                O|
|           ,|                O|
|         and|                O|
|       Buyer|B-WHEREAS_SUBJECT|
|         has| B-WHEREAS_ACTION|
|      agreed| I-WHEREAS_ACTION|
|          to| I-WHEREAS_ACTION|
|    purchase| I-WHEREAS_ACTION|
|         and|                O|
|     acquire|                O|
|           ,|                O|
|         all|                O|
|          of|                O|
|    Seller's|                O|
|       right|                O|
|           ,|                O|
|       title|                O|
|         and|                O|
|    interest|                O|
|          in|                O|
|         and|                O|
|          to|                O|
|   Armstrong|                O|
|        Wood|                O|
|    Products|                O|
|           ,|                O|
|         Inc|                O|
|          .,|                O|
|           a|                O|
|    Delaware|                O|
| corporation|                O|
|          ("|                O|
|         AWP|                O|
|          ")|                O|
|         and|                O|
|         its|                O|
|Subsidiaries|                O|
|           ,|                O|
|         the|                O|
|     Company|                O|
|         and|                O|
|   HomerWood|                O|
|    Hardwood|                O|
|    Flooring|                O|
|     Company|                O|
|           ,|                O|
|           a|                O|
|    Delaware|                O|
| corporation|                O|
|          ("|                O|
|        HHFC|                O|
|          ,"|                O|
+------------+-----------------+
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|legner_whereas|
|Type:|legal|
|Compatibility:|Spark NLP for Legal 1.0.0+|
|License:|Licensed|
|Edition:|Official|
|Input Labels:|[sentence, token, embeddings]|
|Output Labels:|[ner]|
|Language:|en|
|Size:|16.5 MB|

## References

Manual annotations on CUAD dataset

## Benchmarking

```bash
label	 tp	 fp	 fn	 prec	 rec	 f1
B-WHEREAS_SUBJECT	 191	 14	 15	 0.9317073	 0.92718446	 0.9294404
I-WHEREAS_ACTION	 202	 38	 59	 0.84166664	 0.77394634	 0.8063872
I-WHEREAS_SUBJECT	 52	 8	 16	 0.8666667	 0.7647059	 0.8125
B-WHEREAS_OBJECT	 101	 63	 68	 0.61585367	 0.5976331	 0.6066066
B-WHEREAS_ACTION	 152	 19	 16	 0.8888889	 0.9047619	 0.89675516
I-WHEREAS_OBJECT	 361	 194	 194	 0.65045047	 0.65045047	 0.65045047
tp: 1059 fp: 336 fn: 368 labels: 6
Macro-average	 prec: 0.7992056, rec: 0.76978034, f1: 0.784217
Micro-average	 prec: 0.7591398, rec: 0.74211633, f1: 0.75053155
```