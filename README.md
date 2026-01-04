# Emotion-Aware Multilingual Asynchronous Online Learning Analytics: A Multimodal-Multitask Approach using EA-mLaBSE
#### Army Justitia, Hei-Chia Wang

#

This repository contains the code, datasets, and evaluation materials for the study:
**"Emotion-Aware Multilingual Asynchronous Online Learning Analytics: A Multimodal-Multitask Approach using EA-mLaBSE"**

The proposed framework (EA-mLaBSE) performs multilingual, multimodal, and multitask learning for asynchronous online learning feedback analysis by jointly predicting emotion (multilabel), sentiment (multiclass), and aspect (multiclass) from learner-generated feedback, leveraging both textual and emoji-based cues.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

# 1. Dataset
## 1.1 Data Sources
The dataset was collected from two main sources:
**a. Real-world learner feedback (Udemy)**
Learner feedback was gathered from [Udemy](https://www.udemy.com/), a global online learning platform offering courses across diverse domains, including technology, computer science, business, design, and personal development. We focused on computer science courses in six different languages:
- English
- Mandarin Chinese
- Hindi
- Spanish
- French
- Indonesian

To align with the research objectives, we specifically targeted learner feedback that contains emoji usage, as emojis serve as non-verbal emotional signals.
**b. Synthetic feedback using Large Language Models (LLMs)**
To mitigate data sparsity and class imbalance, we generated synthetic feedback using [ChatGPT](https://chatgpt.com/) and [Microsoft Copilot](https://copilot.microsoft.com/). The generation process was conditioned on contextual knowledge derived from original learner feedback. Prompt-level constraints were applied to ensure that the synthetic data remains consistent with the original data distribution in terms of sentiment, aspect, and emotional expressions.

## 1.2 Dataset Characteristics
- Each instance represents **a single learner feedback.**
- If an original feedback contains multiple sentences, it is **split into single-sentence units** using a period (.) as a delimiter.
- Detailed descriptive statistics (data sources, speaker population, course counts, feedback length, emoji usage, and language-specific characteristics) are reported in our paper.

## 1.3	Dataset Directory Structure
### Dataset/4. newlabel/extend/
-	Contains multilingual feedback datasets per language.
-	Besides language-specific files, this folder includes:
	train.csv – training data
	test.csv – testing data  
	Both train.csv and test.csv are concatenations of all language-specific datasets.

### Dataset/4. newlabel/LLMs/
-	Contains LLM-generated labels produced by:
	ChatGPT-4o
	Gemini 2.5
	DeepSeek-V3
	Le Chat (Mistral AI)
	Grok 3 (𝕏AI)
	Microsoft Copilot  
-	Files named **<filename>_metric.xlsx** store performance evaluation results of LLM-based labeling against expert-annotated ground truth.

## 1.4	Evaluation metrics
-	Sentiment classification: Accuracy, Precision, Recall, F1-score  
-	Aspect classification: Accuracy, Precision, Recall, F1-score  
-	Emotion classification:
	Subset Accuracy
	Sample Accuracy
	Micro Precision, Recall, Micro-F1
	Macro Precision, Recall, Macro-F1
	Macro ROC-AUC  

# 2.	Installation and Setup
## 2.1 Prerequisites
Python 3.11.0 or higher is preferred for best compatibility.

## 2.2	Install Dependencies
Ensure you have Python installed (preferably Python 3.10+), then install the required dependencies:
```sh
pip install -r requirements.txt
```
# 3.	Preprocessing
All preprocessing steps are implemented in ``` prep.ipynb```.
## 3.1 Function Cell
Run the Function cell first. It includes:
•	split_hin: splits Hindi feedback into sentence-level units.
•	merge_by_no: reassigns identical course_id, user, rating, and feedback after sentence splitting. (Optional: can be skipped if feedback is already single-sentence.)
•	count_words_mixed: counts code-mixed tokens and non-Latin characters (e.g., Hanzi, Devanagari, Pinyin).
•	find_emoji: extracts emojis and separates them from the text.

This function generates three additional columns:
o	feedback_no_emoji
o	emojis
o	len(emojis)
## 3.2 Emoji Extraction
Run the Extract Emoji cell to:
•	Identify and extract emojis using find_emoji
•	Compute:
    o	Emoji distribution per language
    o	Word-length distribution per language
# 4.	Hyperparameter Optimization with Optuna
We employ an Optuna-based hyperparameter optimization strategy to determine optimal configurations for the EA-mLaBSE framework.The optimization code is provided in ``` OptimizingOptuna.ipynb```
The search space includes:
•	Learning rate
•	Dropout rate
•	Weight decay
•	Warm-up ratio
•	Gradient accumulation steps
•	Loss weighting across tasks
•	Optional focal loss parameters

The optimized hyperparameters are subsequently used for K-Fold training and ensemble inference.
# 5.	Model Training and Evaluation
•	Core model implementation: ```ea_imLaBSE.ipynb```
•	Baseline comparisons: ```Baselines/```
•	Evaluation scripts and outputs: ```Evaluation/```

The framework supports:
•	Multitask learning (emotion, sentiment, aspect)
•	Class imbalance handling (cost-sensitive learning & focal loss)
•	K-Fold cross-validation
•	Ensemble inference via logit averaging
•	Threshold tuning and temperature scaling
# 6.	Reproducibility
This repository is designed to fully support experimental reproducibility.
All preprocessing steps, model configurations, and evaluation procedures are explicitly documented and provided.
Due to GitHub file size limitations, trained model checkpoints are not included in this repository.
Instead, all trained EA-mLaBSE model checkpoints are publicly available on Hugging Face and can be accessed at:
🔗 https://huggingface.co/armyjust/EA-mLaBSE
This separation ensures both reproducibility and efficient distribution of large model files.
# 7.	Citation
If you use this repository, dataset, or code, please cite the corresponding paper (citation details will be updated upon publication).

