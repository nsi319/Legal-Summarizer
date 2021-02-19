---

## LED for legal domain
This is Longformer Encoder Decoder [led-base-16384] model for the legal domain, trained for long document abstractive summarization task. 

## Training data

The legal-led-base-16384 model was trained on [sec-litigation-releases](https://www.sec.gov/litigation/litreleases.htm) dataset consisting more than 4000 litigation releases and complaints.

## Evaluation results

When the model is used for summarizing legal documents, achieves the following results:

Test results :

| Model | Rouge1-Precision  | Rouge1 |
|:-----:|:-----:|:-----:|
|   legal_led-base-16384 | 58.42|54.89 |