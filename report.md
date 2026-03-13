# Week 5 Report: AutoML Training & Fine-Tuned Model Evaluation
**Name:** Abdoul Sawadogo
**Date:** 3/13/2026
**Capstone Project:** Security Alert Triage Bot
**My Component:** Cybersecurity Threat Detection
## Part A: Teachable Machine Training
### Training Setup
- **Task:** Distressed vs Calm facial expression image classification
- **Training images per class:** Distressed: 34 images | Calm: 56 images
- **Test images per class:** 5 per class (10 total, held out before training)
- **Total training time:** ~20 seconds
### Test Results
| # | Actual Class | Predicted Class | Confidence | Correct? |
|---|--------------|-----------------|------------|----------|
| 1 | Calm | Calm | 64% |  Yes |
| 2 | Distressed | Distressed | 98% |  Yes |
| 3 | Calm | Calm | 100% |  Yes |
| 4 | Calm | Calm | 96% |  Yes |
| 5 | Distressed | Distressed | 100% |  Yes |
| 6 | Calm | Calm | 100% |  Yes |
| 7 | Distressed | Distressed | 97% |  Yes |
| 8 | Calm | Calm | 100% |  Yes |
| 9 | Calm | Calm | 99% |  Yes |
| 10 | Distressed | Calm | ~55% |  No |
### Confusion Matrix
| | Predicted: Distressed | Predicted: Calm |
|---|---|---|
| **Actual: Distressed** | TP = 4 | FN = 1 |
| **Actual: Calm** | FP = 0 | TN = 5 |

### Calculated Metrics
- **Accuracy:** 90%
- **Precision:** 100%
- **Recall:** 80%
- **F1 Score:** 88.9%
### Interpretation
The model achieved 100% precision with zero false alarms every face it flagged as distressed was genuinely distressed. However, recall was 80%, meaning one genuinely distressed image was missed and classified as calm. For a forensic psychology context, false negatives (missed distress signals) are more costly than false alarms, since failing to detect a person in distress has more serious real-world consequences. The model would benefit from a more balanced dataset the Distressed class had only 34 training images compared to 56 Calm images, which likely caused a bias toward predicting Calm. Adding 20+ more diverse distressed images would likely improve recall significantly.
## Part B: Generic vs Fine-Tuned Model Comparison
### Models Tested
1. **Generic:** distilbert-base-uncased-finetuned-sst-2-english (sentiment)
2. **Fine-Tuned A:** cybersectony/phishing-email-detection-distilbert_v2.4.1 Phishing vs legitimate email detection
3. **Fine-Tuned B:** mrm8488/bert-tiny-finetuned-sms-spam-detection SMS spam vs ham classification
### Results
| Input | Generic Label (Score) | Fine-Tuned A Label (Score) | Fine-Tuned B Label (Score) | Best Model |
|-------|-----------------------|----------------------------|----------------------------|------------|
| Unauthorized login from IP 198.51.100.4 traced to Moscow | POSITIVE (0.7481) | LABEL_1 / Phishing (0.9997) | LABEL_0 / Ham (0.9256) | Fine-Tuned A |
| Routine firewall rule update completed on fw-01 | POSITIVE (0.7481) | LABEL_1 / Phishing (0.9997) | LABEL_0 / Ham (0.9256) | Fine-Tuned B |
| Phishing email with spoofed Amazon domain detected | POSITIVE (0.7481) | LABEL_1 / Phishing (0.9997) | LABEL_0 / Ham (0.9256) | Fine-Tuned A |
| Multiple failed SSH attempts from Beijing IP | POSITIVE (0.7481) | LABEL_1 / Phishing (0.9997) | LABEL_0 / Ham (0.9256) | Fine-Tuned A |
| System resource utilization normal no anomalies | POSITIVE (0.7481) | LABEL_1 / Phishing (0.9997) | LABEL_0 / Ham (0.9256) | Fine-Tuned B |
### Analysis
**Generic model strengths:** Reliable and easy to integrate into n8n and returned consistent scores 
**Generic model weaknesses:** Classified every cybersecurity input including a confirmed phishing attack and a routine maintenance log identically as "POSITIVE"
**Fine-tuned model advantage:** Fine-Tuned A correctly identified all threat related inputs with very high confidence (0.9997). Fine-Tuned B correctly classified all inputs as legitimate/ham (0.9256), which was appropriate for the routine records. Both provide far more domain-relevant signal than the generic model.
**Biggest surprise:** Fine-Tuned A flagged every single input including "Routine firewall rule update completed" as phishing with 99.97% confidence.
### Recommended Model for My Capstone Component
**Component:** Cybersecurity Threat Detection
**Primary model:** cybersectony/phishing-email-detection-distilbert_v2.4.1 Despite over flagging, it is the only model among the three trained on cybersecurity relevant data and provides more actionable signal than a generic sentiment model. It would be most useful after retraining on a balanced dataset of threat vs. routine security logs.
**Confidence threshold:** 0.95 Since this model consistently produces very high confidence scores regardless of input, a high threshold should be combined with mandatory human review
**Priority metric:** In cybersecurity, missing a real threat (false negative) is far more costly than a false alarm. A missed phishing attack or unauthorized login could result in a data breach, while a false alarm only costs analyst review time. Maximizing recall ensures all real threats are caught.
## Limitations & Next Steps
With only 5 test records and models not trained on security log text (as opposed to email content), these results are preliminary. Fine-Tuned A's inability to distinguish threat alerts from routine logs indicates it was trained on a narrow email dataset rather than the broader cybersecurity alert formats used in production. Given more time, the ideal next step would be to fine-tune a custom model using labeled cybersecurity alert data via Hugging Face AutoTrain, or to test models like jackaduma/SecRoBERTa trained on cybersecurity-specific.
