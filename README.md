# Week 5: AutoML & No-Code Model Training
Trained a custom image classifier with Google Teachable Machine and compared generic vs fine-tuned Hugging Face models for the emotional state detection
of our [capstone project name].
## Custom Model Training
- Built a Distressed/Calm image classifier with Teachable Machine
- Achieved 90% accuracy on 10 held-out test images
- Precision: 75% | Recall: 100% | F1: 85.7%
## Fine-Tuned Model Comparison
Compared 3 models (1 generic + 2 fine-tuned) on 5 test inputs:
- Generic: distilbert-sst-2 (sentiment)
- Fine-Tuned A: cybersectony/phishing-email-detection-distilbert_v2.4.1
- Fine-Tuned B: mrm8488/bert-tiny-finetuned-sms-spam-detection
## Finding
Recommended cybersectony/phishing-email-detection-distilbert_v2.4.1 for emotional state detection because it offers the highest reported accuracy (99.58%) and is purpose-built to detect threatening/malicious communication patterns relevant to forensic risk assessment..
Fine-tuned models showed higher performance with more relevant labels and higher confidence scores.
See `report.md` for full analysis.
