# Customer Support Ticket Tagging with LLMs

## Project Overview
This project explores **different Large Language Model (LLM) approaches** to automatically tag customer support tickets from a free-text dataset.  
The goal was to **compare zero-shot, few-shot, and fine-tuned learning methods** and identify the best-performing one based on evaluation metrics.  
Finally, the best approach was used to output the **top 3 most probable tags for each ticket**.

---

##  Dataset
- **Source:** [Free-text Support Ticket Dataset (Kaggle)](https://www.kaggle.com/datasets/thoughtvector/customer-support-on-twitter) (downloaded via `kagglehub`)
- **Columns:** `tweet_id`, `author_id`, `inbound`, `created_at`, `text`, `response_tweet_id`, `in_response_to_tweet_id`
- **Key Feature:** `text` – the main free-text message used for tagging

---

##  Data Preparation
Steps taken to preprocess the text data:
- Filled missing values with empty strings
- Converted text to **lowercase**
- Removed:
  - URLs
  - Mentions (`@username`)
  - Hashtags (`#hashtag`)
  - Special characters (except alphanumeric and spaces)
- Removed extra whitespace and stripped leading/trailing spaces
- Added a new column `cleaned_text` to store processed text  

 **Result:** Cleaned and consistent dataset, ready for model input.

---

##  Tag Categories
Manually defined 7 support ticket categories after inspecting the dataset:

- **Billing and Payments**
- **Technical Support**
- **Service Issues/Outages**
- **Account Management**
- **General Inquiry**
- **Product Information**
- **Feedback and Suggestions**

Stored in `support_categories` for later use.

---

##  Approaches Explored

### 1️⃣ Zero-shot Learning
- Used LLM prompts without providing any examples
- Classified tickets into top 3 probable tags
- **Evaluation Metrics (Sample of 100 tickets):**
  - **Precision:** `0.2715`
  - **Recall:** `0.4105`
  - **F1-score:** `0.3237`

---

### 2️⃣ Few-shot Learning
- Provided **1-3 labeled examples per category** in the prompt
- Allowed LLM to learn patterns from examples
- **Evaluation Metrics (Sample of 100 tickets):**
  - **Precision:** `0.2773`
  - **Recall:** `0.4293`
  - **F1-score:** `0.3356`  **Best overall performance**

---

### 3️⃣ Fine-tuning a Pre-trained LLM
- Fine-tuned **GPT-2** on 500 labeled samples
- Custom `SupportTicketDataset` created for Hugging Face `Trainer`
- **Evaluation Metrics (Sample of 100 tickets):**
  - **Precision:** `0.2992` (highest among three)
  - **Recall:** `0.1699` (lowest)
  - **F1-score:** `0.2054`

---

## 📊 Results & Comparison

| Approach       | Precision | Recall | F1-score |
|---------------|-----------|--------|----------|
| **Zero-shot** | 0.2715    | 0.4105 | 0.3237   |
| **Few-shot**  | 0.2773    | 0.4293 | **0.3356** ✅ |
| **Fine-tuned**| **0.2992**| 0.1699 | 0.2054   |

 **Interpretation:**
- **Few-shot learning performed best overall** (highest F1-score)
- **Fine-tuning had the best precision** but poor recall, meaning it was too conservative and missed many tags
- **Zero-shot performed reasonably well**, showing the power of LLMs even without examples

---

## Final Output
- **Selected Approach:** **Few-shot Learning**  
- Produced **top 3 predicted tags** for each ticket using the few-shot model
- Displayed results alongside original `cleaned_text` for interpretability

---

##  Key Takeaways
- **Few-shot learning** is an excellent middle ground — no costly model training required, yet improved performance over zero-shot.
- **Fine-tuning** may require more data and compute to outperform few-shot in recall-heavy tasks.
- This notebook serves as a template for experimenting with **LLM-based text classification** pipelines.

---

