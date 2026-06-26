```markdown
# Vietnamese Social Media Sarcasm Detection

This project focuses on **sarcasm detection in Vietnamese social media comments** collected from TikTok and YouTube. Unlike conventional text classification tasks that only use the comment text, this project formulates sarcasm detection as a **context-aware NLP problem**, where each comment is interpreted together with video-level context and commonsense knowledge.

## Project Overview

Sarcasm in Vietnamese social media is difficult to detect because comments are often short, informal, noisy, and highly dependent on video context. A surface-level positive comment may actually express criticism or mockery when placed in the correct context. To address this challenge, this project builds a context-rich dataset and explores multiple modeling strategies for sarcasm classification.

The project supports two main tasks:

- **Binary classification**: Sarcastic vs. Non-Sarcastic
- **Multi-class classification**: Fine-grained sarcasm type prediction, including:
  - Lexical Contradiction
  - Propositional Contradiction
  - Hypothetical
  - Rhetorical Question
  - Wh-Question
  - Non-Sarcastic

## Key Contributions

- Built a Vietnamese social media sarcasm dataset from TikTok and YouTube comments, enriched with video context, commonsense knowledge, sarcasm labels, sarcasm types, and reasoning explanations.
- Designed a context-aware task formulation that combines comment text, textualized video context, and commonsense reasoning instead of relying only on standalone comments.
- Developed a Medallion-style data pipeline with Bronze, Silver, and Gold layers to manage raw data, normalized text, enriched context, and final labeled data.
- Implemented a robust Vietnamese text preprocessing pipeline for noisy social media comments, including Unicode normalization, regex-based cleaning, repeated-character reduction, teencode normalization, emoji demojizing, masking/unmasking, and BARTpho-based error correction.
- Generated commonsense knowledge using LLM prompting with few-shot examples, strict role constraints, lexical blacklisting, and deterministic decoding to reduce label leakage and hallucination.
- Constructed a gold-standard labeled dataset using Qwen 3.7 Max as an LLM-as-a-Judge, applying contextual grounding, hierarchical classification, guided reasoning, and expert prompt engineering.
- Evaluated multiple approaches, including prompting-based LLM inference, traditional and BERT-based baselines, and knowledge distillation from large LLMs to smaller Qwen models.

## Data Processing Pipeline

The data pipeline is organized into three main layers:

### Bronze Layer

The Bronze layer stores raw social media comments collected from TikTok and YouTube. At this stage, the data may still contain noise such as timestamps, emojis, teencode, repeated characters, informal spelling, and sticker-only comments.

### Silver Layer

The Silver layer applies text normalization and context enrichment. The preprocessing pipeline includes:

- Unicode NFC normalization
- Regex-based punctuation and noise cleaning
- Repeated-character reduction, e.g., `koooo` → `ko`
- OOV extraction using a Vietnamese word list
- LLM-assisted teencode dictionary generation
- Emoji demojizing to preserve emotional signals
- Rule-based teencode normalization
- Masking protected entities such as emojis, English words, loanwords, and named entities
- BARTpho-based Vietnamese error correction
- Unmasking to restore protected tokens

This layer produces cleaned and normalized comments while preserving the original meaning and social media language characteristics.

### Gold Layer

The Gold layer contains the final enriched and labeled dataset. Each sample includes:

- Comment text
- Platform information
- Video ID and video metadata
- Video core content
- Commonsense knowledge
- Sarcasm label
- Sarcasm type
- Reasoning explanation

This final dataset is used for model training, evaluation, prompting experiments, and knowledge distillation.

## LLM-based Labeling

The project uses an LLM-as-a-Judge strategy to automatically label sarcasm in Vietnamese social media comments. Qwen 3.7 Max is used as the main teacher model to assign:

- `sarcasm_label`: Sarcastic or Non-Sarcastic
- `sarcasm_type`: fine-grained sarcasm category
- `reasoning`: a short explanation for the decision

To improve labeling reliability, the prompt design includes:

- Contextual grounding with comment and video context as primary evidence
- Commonsense knowledge as auxiliary information only
- Hierarchical classification from binary sarcasm detection to fine-grained sarcasm type classification
- Guided reasoning rules for identifying incongruity between surface meaning and video context
- Few-shot contrastive examples to clarify decision boundaries
- Strict constraints to reduce hallucination and label leakage

## Modeling Approaches

The project evaluates multiple modeling strategies:

### Prompting-based LLM Inference

Large language models are evaluated using:

- Zero-shot prompting
- Few-shot prompting
- Chain-of-Thought prompting
- Combined few-shot and reasoning prompts

### Traditional and BERT-based Baselines

The project implements baseline models including:

- SVM with TF-IDF features
- PhoBERT
- XLM-RoBERTa

These models are used to evaluate the difficulty of sarcasm detection under both binary and multi-class settings.

### Knowledge Distillation

To transfer reasoning ability from a large teacher model to smaller models, the project applies supervised fine-tuning and knowledge distillation from Qwen 3.7 Max to smaller Qwen models. The distilled models are trained to reproduce structured JSON outputs containing reasoning, sarcasm labels, and sarcasm types.

## Challenges

Vietnamese sarcasm detection on social media remains challenging due to:

- Short and context-dependent comments
- Teencode, slang, emoji, and informal spelling
- Rapidly changing Vietnamese internet language
- Ambiguity between jokes, teasing, criticism, and sarcasm
- Severe class imbalance between Non-Sarcastic and Sarcastic samples
- Hallucination or over-reasoning when video context or commonsense knowledge is incomplete

## Project Goal

The goal of this project is not only to train a classifier, but also to build a complete data-centric NLP pipeline for Vietnamese sarcasm detection. The system combines data preprocessing, context enrichment, LLM-based labeling, model training, error analysis, and knowledge distillation to better understand how sarcasm appears in Vietnamese social media comments.
```
