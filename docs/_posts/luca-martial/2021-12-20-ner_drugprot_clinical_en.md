---
layout: model
title: Detect Drugs and Proteins
author: John Snow Labs
name: ner_drugprot_clinical
date: 2021-12-20
tags: [ner, clinical, drugprot, en, licensed]
task: Named Entity Recognition
language: en
edition: Spark NLP for Healthcare 3.3.4
spark_version: 3.0
supported: true
article_header:
type: cover
use_language_switcher: "Python-Scala-Java"
---


## Description


This model detects chemical compounds/drugs and genes/proteins in medical text and research articles. Chemical compounds/drugs are labeled as `CHEMICAL`, genes/proteins are labeled as `GENE` and entity mentions of type `GENE` and of type `CHEMICAL` that overlap such as enzymes and small peptides are labeled as `GENE_AND_CHEMICAL`.


## Predicted Entities


`GENE`, `CHEMICAL`, `GENE_AND_CHEMICAL`


{:.btn-box}
[Live Demo](https://demo.johnsnowlabs.com/healthcare/NER_DRUG_PROT/){:.button.button-orange}
[Open in Colab](https://github.com/JohnSnowLabs/spark-nlp-workshop/blob/master/tutorials/Certification_Trainings/Healthcare/1.Clinical_Named_Entity_Recognition_Model.ipynb){:.button.button-orange.button-orange-trans.co.button-icon}
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/clinical/models/ner_drugprot_clinical_en_3.3.3_3.0_1639989110299.zip){:.button.button-orange.button-orange-trans.arr.button-icon}


## How to use






<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}

```python
...
embeddings_clinical = WordEmbeddingsModel.pretrained("embeddings_clinical", "en", "clinical/models")\
.setInputCols(["sentence", "token"])\
.setOutputCol("embeddings")

clinical_ner = MedicalNerModel.pretrained("ner_drugprot_clinical", "en", "clinical/models")\
.setInputCols(["sentence", "token", "embeddings"])\
.setOutputCol("ner")

nlpPipeline = Pipeline(stages=[document_assembler, sentence_detector, tokenizer, embeddings_clinical, clinical_ner, ner_converter])

model = nlpPipeline.fit(spark.createDataFrame([[""]]).toDF("text"))

EXAMPLE_TEXT = "Anabolic effects of clenbuterol on skeletal muscle are mediated by beta 2-adrenoceptor activation."

results = model.transform(spark.createDataFrame([[EXAMPLE_TEXT]]).toDF("text"))
```
```scala
...
val embeddings_clinical = WordEmbeddingsModel.pretrained("embeddings_clinical", "en", "clinical/models")
.setInputCols(Array("sentence", "token"))
.setOutputCol("embeddings")

val ner = MedicalNerModel.pretrained("ner_drugprot_clinical", "en", "clinical/models")
.setInputCols(Array("sentence", "token", "embeddings"))
.setOutputCol("ner")

val pipeline = new Pipeline().setStages(Array(document_assembler, sentence_detector, tokenizer, embeddings_clinical, ner, ner_converter))

val data = Seq("""Anabolic effects of clenbuterol on skeletal muscle are mediated by beta 2-adrenoceptor activation.""").toDS.toDF("text")

val result = pipeline.fit(data).transform(data)
```


{:.nlu-block}
```python
import nlu
nlu.load("en.med_ner.drugprot_clinical").predict("""Anabolic effects of clenbuterol on skeletal muscle are mediated by beta 2-adrenoceptor activation.""")
```

</div>


## Results


```bash
+-------------------------------+---------+
|chunk                          |ner_label|
+-------------------------------+---------+
|clenbuterol                    |CHEMICAL |
|beta 2-adrenoceptor            |GENE     |
+-------------------------------+---------+
```


{:.model-param}
## Model Information


{:.table-model}
|---|---|
|Model Name:|ner_drugprot_clinical|
|Compatibility:|Spark NLP for Healthcare 3.3.4+|
|License:|Licensed|
|Edition:|Official|
|Input Labels:|[sentence, token, embeddings]|
|Output Labels:|[ner]|
|Language:|en|
|Size:|14.7 MB|
|Dependencies:|embeddings_clinical|


## Data Source


This model was trained on the [DrugProt corpus](https://zenodo.org/record/5119892).


## Benchmarking


```bash
label      tp     fp     fn   total  precision  recall   f1
GENE_AND_CHEMICAL   786.0  171.0  143.0   929.0     0.8213  0.8461   0.8335
CHEMICAL  8228.0  779.0  575.0  8803.0     0.9135  0.9347   0.924
GENE  7176.0  822.0  652.0  7828.0     0.8972  0.9167   0.9069
macro      -       -      -       -       -        -     0.88811683
micro      -       -      -       -       -        -     0.91156048
```
