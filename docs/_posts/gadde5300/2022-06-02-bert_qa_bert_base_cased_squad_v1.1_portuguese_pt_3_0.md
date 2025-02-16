---
layout: model
title: Portuguese BertForQuestionAnswering model (from pierreguillou)
author: John Snow Labs
name: bert_qa_bert_base_cased_squad_v1.1_portuguese
date: 2022-06-02
tags: [pt, open_source, question_answering, bert]
task: Question Answering
language: pt
edition: Spark NLP 4.0.0
spark_version: 3.0
supported: true
article_header:
type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained Question Answering model, adapted from Hugging Face and curated to provide scalability and production-readiness using Spark NLP. `bert-base-cased-squad-v1.1-portuguese` is a Portuguese model orginally trained by `pierreguillou`.

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/bert_qa_bert_base_cased_squad_v1.1_portuguese_pt_4.0.0_3.0_1654179838573.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
document_assembler = MultiDocumentAssembler() \ 
.setInputCols(["question", "context"]) \
.setOutputCols(["document_question", "document_context"])

spanClassifier = BertForQuestionAnswering.pretrained("bert_qa_bert_base_cased_squad_v1.1_portuguese","pt") \
.setInputCols(["document_question", "document_context"]) \
.setOutputCol("answer") \
.setCaseSensitive(True)

pipeline = Pipeline().setStages([
document_assembler,
spanClassifier
])

example = spark.createDataFrame([["What's my name?", "My name is Clara and I live in Berkeley."]]).toDF("question", "context")

result = pipeline.fit(example).transform(example)
```
```scala
val document = new MultiDocumentAssembler()
.setInputCols("question", "context")
.setOutputCols("document_question", "document_context")

val spanClassifier = BertForQuestionAnswering
.pretrained("bert_qa_bert_base_cased_squad_v1.1_portuguese","pt")
.setInputCols(Array("document_question", "document_context"))
.setOutputCol("answer")
.setCaseSensitive(true)
.setMaxSentenceLength(512)

val pipeline = new Pipeline().setStages(Array(document, spanClassifier))

val example = Seq(
("Where was John Lenon born?", "John Lenon was born in London and lived in Paris. My name is Sarah and I live in London."),
("What's my name?", "My name is Clara and I live in Berkeley."))
.toDF("question", "context")

val result = pipeline.fit(example).transform(example)
```


{:.nlu-block}
```python
import nlu
nlu.load("pt.answer_question.squad.bert.base_cased.by_pierreguillou").predict("""What's my name?|||"My name is Clara and I live in Berkeley.""")
```

</div>

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|bert_qa_bert_base_cased_squad_v1.1_portuguese|
|Compatibility:|Spark NLP 4.0.0+|
|License:|Open Source|
|Edition:|Official|
|Input Labels:|[sentence, token]|
|Output Labels:|[embeddings]|
|Language:|pt|
|Size:|406.5 MB|
|Case sensitive:|true|
|Max sentence length:|512|

## References

- https://huggingface.co/pierreguillou/bert-base-cased-squad-v1.1-portuguese
- https://colab.research.google.com/
- https://ailab.unb.br/
- https://medium.com/@pierre_guillou/nlp-modelo-de-question-answering-em-qualquer-idioma-baseado-no-bert-base-estudo-de-caso-em-12093d385e78
- https://www.linkedin.com/in/pierreguillou/
- https://medium.com/@pierre_guillou/nlp-modelo-de-question-answering-em-qualquer-idioma-baseado-no-bert-base-estudo-de-caso-em-12093d385e78#c572
- http://www.deeplearningbrasil.com.br/
- https://neuralmind.ai/
- https://colab.research.google.com/drive/18ueLdi_V321Gz37x4gHq8mb4XZSGWfZx?usp=sharing
- https://github.com/piegu/language-models/blob/master/colab_question_answering_BERT_base_cased_squad_v11_pt.ipynb