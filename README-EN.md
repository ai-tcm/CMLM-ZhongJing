<div align="center">

# 🏥 CMLM-ZhongJing (The First Traditional Chinese Medicine Large Language Model)

### *An Expert Knowledge-Guided Language Model for Traditional Chinese Medicine*

[中文](https://github.com/pariskang/CMLM-ZhongJing/blob/main/README.md) | [English](https://github.com/pariskang/CMLM-ZhongJing/blob/main/README-EN.md)

[![Paper](https://img.shields.io/badge/📄_Paper-Tsinghua_Science_&_Technology-blue)](https://doi.org/10.26599/TST.2025.9010046)
[![HuggingFace](https://img.shields.io/badge/🤗_HuggingFace-Models-yellow)](https://huggingface.co/CMLM/ZhongjingGPT1_13B)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1DACpocPh9kMVeify-2_fuooOS3RjVgNc)
[![License](https://img.shields.io/badge/License-Academic_Use-green)]()

<img src="https://raw.githubusercontent.com/pariskang/CMLM-ZhongJing/main/logo.png" alt="ZhongJing Logo" width="40%">

> In May 2023, we trained the first Traditional Chinese Medicine (TCM) foundation model—**ZhongJingGPT**, inspired by the wisdom of **Zhang Zhongjing**, one of ancient China's most distinguished physicians. This model aims to elucidate the profound knowledge of TCM, inheriting ancient wisdom while driving modern technological innovation, providing a trustworthy and professional tool for the medical field.

**⚠️ DISCLAIMER: All outputs are currently for academic research reference only and do not constitute any medical advice. Clinical diagnosis and treatment must be provided by experienced professional physicians.**

</div>

---

## 📢 Latest Updates

| Date | Update |
|------|--------|
| **2025.05.08** | 🎉 Paper **"ZhongJingGPT: An Expert Knowledge-Guided Language Model for Traditional Chinese Medicine"** accepted by *Tsinghua Science and Technology*! [Read Paper →](https://doi.org/10.26599/TST.2025.9010046) |

---

## 🌟 Project Highlights

- 🧠 **Multi-Task Diagnostic Decomposition Strategy** — Drawing from human memory and learning mechanisms, constructing high-quality instruction data through 15 diagnostic and therapeutic scenarios
- 📚 **135,000+ Professional Instructions** — Covering TCM classics, formulas, syndromes, tongue and pulse diagnosis, critical thinking, and other multi-dimensional knowledge
- 🩺 **Cross-Specialty Generalization** — Trained on gynecology data, demonstrates strong diagnostic and prescription capabilities across internal medicine, surgery, orthopedics, and other disciplines
- 🔬 **Human Physician Validation** — Systematically evaluated by five professional physicians across five dimensions
- ⚡ **Lightweight Deployment** — 1.8B model achieves high-speed inference on a single Tesla T4 GPU

---

## 📦 Model Downloads

We have open-sourced fine-tuned weights for **Baichuan2-13B-Chat** and **Qwen1.5-1.8B-Chat**, ensuring strong understanding and generation capabilities in the TCM domain through multi-round iterative training on proprietary medical datasets.

| Version | Parameters | Base Model | Download Link | Requirements |
|:---:|:---:|:---:|:---:|:---:|
| **ZhongjingGPT1_13B** | 13B | Baichuan2-13B-Chat | [🤗 HuggingFace](https://huggingface.co/CMLM/ZhongjingGPT1_13B) | High-end GPU |
| **ZhongJing-2-1_8b** | 1.8B | Qwen1.5-1.8B-Chat | [🤗 HuggingFace](https://huggingface.co/CMLL/ZhongJing-2-1_8b) | Single T4 GPU |

> 💡 **Quick Experience**: Try the 1.8B model with free GPU inference on [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1DACpocPh9kMVeify-2_fuooOS3RjVgNc), no local deployment required.

---

## 🚀 Quick Start

### Web Demo

We provide a Gradio-based web demo supporting both **single-turn** and **multi-turn** conversations:

```bash
# 1. Clone the repository
git clone https://github.com/pariskang/CMLM-ZhongJing.git
cd CMLM-ZhongJing

# 2. Launch Web Demo
python WebDemo.py
```

---

## 🔬 Technical Approach

### 1. Instruction Data Construction

Current Self-Instruct methods (e.g., Alpaca, Belle) can rapidly construct massive instruction datasets, but in domains with extremely low tolerance for professional errors like **medicine and law**, hallucinated outputs severely impact model accuracy—improper diagnosis and prescription recommendations can even endanger patient lives.

We propose a **professionalism-centered** instruction data construction method:

<p align="center">
<img src="https://raw.githubusercontent.com/pariskang/CMLM-ZhongJing/main/logo_image/Strategy.jpeg" alt="strategy" width="90%">
</p>
<p align="center"><b>Figure 1. Multi-Task Diagnostic Decomposition Strategy with Human Physician Involvement</b></p>

#### 1.1 Multi-Task Diagnostic Decomposition Strategy

Drawing from how humans memorize knowledge—interleaved learning, spaced practice, and diversified learning enhance knowledge consolidation. We use professional tables and leverage large language models' linguistic representation capabilities with carefully designed prompt templates to generate instruction data covering **15 diagnostic scenarios** based on TCM gynecology formula data:

<div align="center">

`Patient Treatment Stories` · `Diagnostic Analysis` · `Expected Treatment Outcomes` · `Formula Functions` · `Interactive Stories` · `Narrative Medicine` · `Tongue and Pulse Diagnosis` · `Treatment Planning` · `Critical Thinking` · `Follow-up` · `Prescriptions` · `Herb Dosage` · `Case Studies` · `Real-World Problems` · `Disease Mechanisms`

</div>

<details>
<summary>📋 <b>Instruction Examples (Click to Expand)</b></summary>

**Multi-Task Decomposition Instruction Example:**
```json
{
    "instruction": "I'm curious about the complete process of Sanyuan Tang. Could you provide a detailed introduction covering aspects such as introduction, medical case, symptoms, diagnosis and treatment, and discussion of results?",
    "input": "",
    "output": "Sanyuan Tang is an ancient Chinese medicine prescription, first appearing in the 'Baomin Collection' volume. It consists of multiple Chinese herbs and is primarily used to treat postpartum prolonged deficiency fatigue, rapid floating pulse, and other symptoms..."
}
```

**TCM General Knowledge Instruction Example:**
```json
{
    "instruction": "Please answer questions related to TCM disease terminology:",
    "input": "What category does (Lung Wind) acne belong to?",
    "output": "Caused by lung wind, stomach heat, or liver stagnation. A skin disease characterized primarily by blackheads or whiteheads, papules, pustules, nodules, cysts, and scars on the face and back."
}
```
</details>

#### 1.2 TCM General Knowledge Instructions

Additionally, we constructed general knowledge instruction data (approximately 80,000 entries) covering TCM classics, term explanations, symptom synonyms/antonyms, syndromes, symptoms, treatment methods, and other multi-dimensional knowledge as a control experiment, using only a single instruction template to represent the data.

<details>
<summary>📊 <b>Complete Dataset Statistics (Click to Expand)</b></summary>

| Data File | Token Count | Instructions |
|:---|---:|---:|
| Ancient Books Content | 15,971,297 | 31,395 |
| Chinese Medicine Symptom Synonyms | 1,515,796 | 27,650 |
| Chinese Medicine Dictionary | 2,188,672 | 20,376 |
| real_world_problem | 1,493,551 | 7,990 |
| disease_mechanism | 997,377 | 8,024 |
| diagnostic_analysis | 1,492,105 | 6,592 |
| follow_up_data | 504,717 | 5,990 |
| herb_dosage | 564,394 | 5,973 |
| therapeutic_template_making | 335,602 | 4,929 |
| tongue_palse | 328,597 | 3,723 |
| prescription_data | 107,694 | 2,898 |
| formula_funtion_data | 100,533 | 2,115 |
| Synonyms2 | 111,186 | 2,217 |
| Treatment Noun Explanation | 81,211 | 1,123 |
| Syndrome Noun Explanation | 67,443 | 976 |
| patient_therapeutic_story (×3) | 164,292 | 1,528 |
| Gynecology Synonyms | 29,740 | 543 |
| case_study_data | 58,319 | 243 |
| critical_thinking_data | 31,502 | 229 |
| Interactive Story | 55,262 | 219 |
| narrative_medicine | 61,336 | 213 |
| diagnosis_treatment_expected_result | 33,822 | 153 |
| Antonyms | 272 | 9 |
| **TOTAL** | **~26,294,720** | **135,108** |

</details>

---

### 2. Model Evaluation

We systematically evaluated model capabilities across three dimensions: **routine TCM symptom consultation**, **complex diagnostic decision reasoning**, and **general Western medical Q&A**. Comparison models include GPT-4, BaiChuan2-13B-Chat, Baichuan 2 53B, and DeepSeek-V3.2.

> 📌 **Key Finding**: In specialist physician case tests, ZhongJing demonstrates TCM syndrome differentiation and prescription capabilities **superior to GPT-4**, and in complex diagnostic decision reasoning shows syndrome differentiation and prescription levels **comparable to national master physicians**.

**⚠️ All model outputs below are for academic comparison research only and do not constitute medical advice.**

---

#### 📋 Test Case 1: Routine TCM Symptom Consultation

> **Q**: Heart pain radiating to back, back pain radiating to heart, how to treat? Please provide TCM prescription for these symptoms.

<details>
<summary>💬 <b>GPT-4</b> — <kbd>Poor Prescription Decisions</kbd></summary>

The symptom description "heart pain radiating to back, back pain radiating to heart" may involve pain in both the heart and back. In TCM concepts, this may be related to the flow of "qi" and "blood," and may also involve the interrelationship between the heart and other internal organs. It's important to note that seeking professional medical diagnosis is crucial before addressing any pain or illness, as chest and back pain can both be signs of serious health problems (such as heart disease). Within the TCM framework, methods for addressing this type of pain may include herbs, acupuncture, cupping, massage (tuina), and other treatment methods. Below is a very generalized example and is not a specific prescription. It's strongly advised that TCM treatment usually requires personalized adjustment based on individual constitution, so it's highly recommended to consult and receive guidance from a professional TCM physician before taking any treatment measures. General herbal recommendations (for reference only): Salvia miltiorrhiza: typically used to activate blood circulation, remove blood stasis, and relieve heart and chest pain. Ligusticum chuanxiong: commonly used to activate blood circulation, remove blood stasis, and relieve pain. Prunus persica seed: commonly used to activate blood circulation, remove blood stasis, and reduce pain. Acupuncture: may be performed on some key acupoints, such as: - Xinshu: on the back, an acupoint related to the heart, commonly used to relieve heart and back pain. - Shenque: located on the abdomen, used for various diseases related to internal organ function. - Neiguan: on the forearm, related to the treatment of heart and chest pain. Massage and tuina: TCM therapists may try to improve the flow of qi and blood and relieve pain through specific massage and tuina techniques. Please be sure to remember that the above information is only general reference and is not individualized medical advice. Any TCM treatment and herbal use should be conducted under the guidance of a professional TCM physician. If you or others experience severe or persistent chest pain, especially if this pain spreads to the back, neck, jaw, left arm, or shoulder, please seek medical help immediately, as this may be a sign of heart problems.
</details>

<details>
<summary>💬 <b>BaiChuan2-13B-Chat</b> — <kbd>TCM Knowledge Gaps</kbd></summary>

I'm not a doctor, but I can provide some suggestions. Before seeking professional medical advice, you can try the following methods to relieve pain: 1. Hot or cold compress: Using a hot water bottle or ice pack on the painful area can help relieve pain and muscle tension. 2. Massage: Gently massaging the painful area can help relax muscles and reduce pain. 3. Adjust sitting posture: Make sure your sitting posture is correct and avoid maintaining the same posture for a long time. 4. Rest: Give injured muscles enough rest time to recover and repair damaged tissue. 5. Medication: Use over-the-counter pain relievers such as Ibuprofen or Acetaminophen to relieve pain. But please consult a doctor or pharmacist's advice before use. As for TCM prescriptions, you can consider the following possible treatment methods: 1. Acupuncture: Regulate the body's qi and blood circulation by inserting fine needles at specific acupoints to achieve pain relief. 2. Cupping: Place cups on the painful area, using negative pressure suction to stimulate meridians and acupoints, thereby relieving pain. 3. Chinese herbal therapy: Based on your constitution and condition, the doctor may prescribe targeted Chinese herbal decoctions, pills, or ointments to regulate the body and achieve pain relief. Please be sure to consult a professional TCM physician to formulate an appropriate treatment plan based on your specific situation.
</details>

<details>
<summary>💬 <b>Baichuan 2 53B</b> — <kbd>Inappropriate Pathogenesis</kbd></summary>

The symptoms you described are very serious heart disease symptoms, possibly manifestations of angina or myocardial infarction. This is an emergency situation that requires immediate hospital examination and treatment. In this situation, self-treatment is very dangerous because if not treated promptly and correctly, it may lead to life-threatening danger. In TCM, such symptoms are usually considered heart-kidney disharmony, requiring nourishing yin, reducing fire, nourishing the heart, and calming the spirit. However, this is only theoretical explanation and cannot replace modern medical diagnosis and treatment. Therefore, I strongly recommend that you immediately go to the hospital for further examination and treatment, and do not attempt self-treatment.
</details>

<details>
<summary>💬 <b>DeepSeek-V3.2</b></summary>

Heart pain radiating to back, back pain radiating to heart—this symptom is commonly seen in the category of chest bi-syndrome with heart pain in TCM, often related to heart yang deficiency and phlegm-stasis obstruction. The pathogenesis commonly involves obstruction of heart vessels, qi-blood stagnation, with pain radiating to the back. The following analyzes from a TCM differentiation perspective and provides reference formulas:

**I. Key Points of Differentiation**
- Cold congealing heart vessels: aggravated by cold, body cold with cold limbs, pale tongue with white coating.
- Phlegm turbidity obstruction: chest oppression and stuffiness, excessive phlegm with shortness of breath, greasy coating.
- Blood stasis blocking collaterals: stabbing pain at fixed location, purple dark tongue or ecchymosis.
- Heart qi deficiency: palpitations with shortness of breath, fatigue with spontaneous sweating, aggravated by movement.

**II. Reference Formulas (Must be used under physician differentiation)** Base formula: Gualou Xiebai Banxia Tang combined with Xuefu Zhuyu Tang with modifications
- Gualou Pi 15g, Xiebai 12g, Fa Banxia 9g (transform phlegm and expand chest)
- Danshen 15g, Chuanxiong 9g, Taoren 9g, Honghua 6g (activate blood and dissolve stasis)
- Guizhi 6g, Tanxiang 6g (warm and unblock heart yang)
- Yanhusuo 9g, Sanqi powder 3g (take with decoction) (dissolve stasis and stabilize pain)
- Zhi Gancao 6g (harmonize all herbs)

**III. Modifications Based on Symptoms**
- Heavy cold: add Zhi Fuzi 6g (decoct first), Ganjiang 6g.
- Excessive phlegm-dampness: add Fuling 15g, Chenpi 9g.
- Obvious qi deficiency: add Huangqi 20g, Dangshen 15g.
- Insomnia and palpitations: add Suanzaoren 15g, Yuanzhi 9g.

**IV. Auxiliary Therapies**
- Acupuncture: select Neiguan, Danzhong, Xinshu, Jueyinshu and other points, even supplementation and drainage.
- External treatment: Rougui, Wuzhuyu equal parts ground into powder, vinegar application to precordial area (use cautiously with sensitive skin).
- Life regulation: avoid cold and keep warm, avoid eating raw, cold, greasy foods. Maintain emotional stability, avoid fatigue. Can appropriately practice Eight Brocades, Tai Chi.

**V. Important Reminders**
- Emergency situations: If pain is severe, continuous and does not relieve, or accompanied by cold sweats, difficulty breathing, seek immediate medical attention to rule out critical conditions such as acute myocardial infarction.
- Differentiation premise: This formula is a general approach, specific medication needs adjustment after TCM physician's four diagnostic methods.
- Integration of TCM and Western medicine: Modern medical diagnosis (such as ECG, coronary CTA) is indispensable, do not stop medication on your own.
</details>

**✅ ZhongJingGPT-13B** — <kbd>🏅 Clear Pathogenesis Explanation, Rational Prescription</kbd>

> Patients with heart pain radiating to back and back pain radiating to heart mostly fall under the category of **chest bi-syndrome**. TCM considers chest bi-syndrome caused by deficiency of vital qi leading to obstruction of heart vessels by phlegm turbidity, blood stasis, and other pathogenic factors. Therefore, treatment focuses on **activating blood and dissolving stasis, eliminating phlegm and unblocking collaterals**.
>
> Commonly used Chinese medicines include:
>
> **1. Danshen Yin (Salvia Beverage)**: Danshen, Tanxiang, Sharen. Among them, Danshen activates blood and dissolves stasis; Tanxiang moves qi and relieves pain; Sharen transforms dampness. The three used together achieve blood activation, stasis dissolution, qi movement, and pain relief.
>
> **2. Xuefu Zhuyu Tang (Blood Palace Stasis Expelling Decoction)**: Taoren, Honghua, Danggui, Shengdi, Chuanxiong, Chishao, Niuxi, Jiegeng, Zhiqiao, Gancao. Among them, Taoren, Honghua, Danggui, Chuanxiong, Chishao activate blood and dissolve stasis; Shengdi clears heat and cools blood; Niuxi guides drugs downward; Jiegeng opens lung qi and regulates water passages; Zhiqiao regulates qi and expands chest; Gancao harmonizes all herbs.

| Model | Syndrome Differentiation | Prescription Quality | Overall Assessment |
|:---|:---:|:---:|:---:|
| GPT-4 | ⚠️ Generic | ❌ No complete formula | Poor prescription decisions |
| BaiChuan2-13B | ❌ TCM knowledge gaps | ❌ No prescription | Bias toward Western medicine symptom management |
| Baichuan 2 53B | ❌ Inappropriate pathogenesis | ❌ No prescription | Safety-oriented primarily |
| DeepSeek-V3.2 | ✅ Comprehensive system | ✅ Complete formula | Detailed differentiation |
| **ZhongJingGPT** | **✅ Concise & accurate** | **✅ Classic formulas** | **Precise pathogenesis, rational prescription** |

---

#### 📋 Test Case 2: Complex Diagnostic Decision Reasoning

> **Q**: Liao, 46 years old, adult. From Loudi, Hunan. Patient presented for proteinuria and hematuria discovered over 10 years. 10 years ago due to edema, hospital examination revealed "proteinuria and hematuria," diagnosed as "chronic nephritis." Despite treatment, condition gradually worsened, blood pressure elevated. Current proteinuria persists (+++) occult blood (+), serum creatinine and urea nitrogen both elevated. Western medical diagnosis: "chronic nephritis, renal insufficiency." Current symptoms: spontaneous sweating, dizziness, lower back pain, fatigue, restless sleep, yellow urine. Red tongue with thin yellow coating, thin rapid pulse. Please provide diagnosis and TCM prescription.

**🏆 Reference Standard — Master Xiong Jibo's Medical Case**

| Item | Content |
|:---|:---|
| **Differentiation** | Yin deficiency with fire effulgence combined with qi deficiency |
| **Treatment Principle** | Nourish yin, reduce fire, and supplement qi |
| **Formula Selection** | Zhibai Dihuang Tang combined with Huangqi Longmu San |
| **Prescription** | Huangqi 40g, Calcined Longgu 30g, Calcined Muli 30g, Shudihuang 15g, Huai Shanyao 15g, Fuling 15g, Zexie 10g, Danpi 10g, Zaoyi 10g, Huangbai 10g, Zhimu 10g, Yumixu 10g, Chuan Niuxi 15g, Fried Yiyiren 20g. 30 doses, water decoction. |
| **Analysis** | Chronic kidney disease mostly belongs to deficiency patterns, primarily kidney deficiency, qi deficiency, blood deficiency, with some combined with spleen deficiency. However, deficiency often combines with excess, some with water-dampness, some with heat, some with cold, must be differentiated and treated. |

<details>
<summary>💬 <b>GPT-4</b> — <kbd>Imprecise Differentiation</kbd></summary>

First, I must emphasize I'm not a doctor... TCM diagnosis: Based on symptoms (spontaneous sweating, dizziness, lower back pain, fatigue, restless sleep, yellow urine, red tongue with thin yellow coating, thin rapid pulse) can consider diagnosis as "kidney yang deficiency" or "kidney yin deficiency," which may also be related to "damp-heat internal accumulation"... TCM prescription for reference only: Shudihuang, Shanyao, Duzhong... (continued below).
</details>

<details>
<summary>💬 <b>Baichuan 2 53B</b> — <kbd>Poor Prescription Approach</kbd></summary>

Liao, 46 years old, adult... (repeats case history)... From a TCM perspective, the patient's symptoms manifest as kidney deficiency with damp-heat internal accumulation. TCM prescription as follows: Shengdihuang 30g, Shanyao 20g, Shanzhuyu 15g, Fuling 15g, Zexie 15g, Danpi 15g, Guizhi 10g, Fuzi 10g, Cheqianzi 15g, Shiwei 15g, Qumai 15g, Huashi 15g, Gancao 10g. Usage: Water decoction, 1 dose daily, take morning and evening.
</details>

<details>
<summary>💬 <b>DeepSeek-V3.2</b></summary>

**TCM Diagnosis**: Urinary turbidity (chronic nephritis, renal insufficiency)

**Syndrome Type**: Kidney yin deficiency, damp-heat internal accumulation, combined with qi deficiency and blood stasis

**Differentiation Analysis**: Patient's prolonged illness, kidney yin deficiency, lower back region loses nourishment, hence lower back pain, fatigue; yin deficiency generates internal heat, heat disturbs heart spirit causing restless sleep; damp-heat descending, scorching blood vessels, hence proteinuria, hematuria, yellow urine; damp-heat rising to disturb clear orifices, plus yin deficiency yang hyperactivity, hence dizziness; qi deficiency with unstable exterior causing spontaneous sweating; red tongue with thin yellow coating, thin rapid pulse are signs of yin deficiency with damp-heat; thin rapid pulse is a sign of yin deficiency with internal heat.

**Treatment Principle**: Supplement kidney yin, clear heat and drain dampness, supplemented with qi supplementation and stasis transformation

**Prescription**: Zhibai Dihuang Wan combined with Liuwei Dihuang Wan with modifications

| Herb | Dosage | Function |
|:---|:---:|:---|
| Shudihuang | 15g | Supplement kidney yin, secure essence |
| Shanzhuyu | 12g | Supplement kidney yin, secure essence |
| Shanyao | 15g | Supplement kidney yin, secure essence |
| Zexie | 10g | Drain dampness and turbidity, clear deficiency heat |
| Fuling | 15g | Drain dampness and turbidity, clear deficiency heat |
| Mudanpi | 10g | Drain dampness and turbidity, clear deficiency heat |
| Zhimu | 10g | Clear heat and dry dampness, nourish yin and reduce fire |
| Huangbai | 10g | Clear heat and dry dampness, nourish yin and reduce fire |
| Huangqi | 20g | Supplement qi and secure exterior, drain water and reduce edema |
| Danshen | 15g | Activate blood and transform stasis, unblock collaterals and drain water |
| Yimucao | 15g | Activate blood and transform stasis, unblock collaterals and drain water |
| Baimaogen | 30g | Clear heat and cool blood, diuretic and unblock strangury |
| Qianshi | 15g | Secure kidney and bind essence, reduce proteinuria |
| Jinyingzi | 15g | Secure kidney and bind essence, reduce proteinuria |

Decoction method: 1 dose daily, water decoction, warm divided into 2 servings, take for 14 consecutive doses. During medication avoid spicy, greasy foods, rest, avoid fatigue.

Care recommendations: 1. Low salt, low protein diet, control blood pressure; 2. Regular follow-up urine routine, kidney function; 3. Maintain emotional stability, avoid external contraction.

Note: This prescription is a TCM differentiation treatment reference, specific medication needs to be combined with clinical practice, use under guidance of practicing physician.
</details>

**✅ ZhongJingGPT-13B** — <kbd>🏅 Comparable to National Master Physician Differentiation and Prescription</kbd>

> Based on the patient's medical history and symptoms, combined with tongue and pulse signs, can diagnose as "chronic glomerulonephritis, decompensated renal insufficiency." The pathogenesis is **damp-heat internal accumulation, prolonged consumption of qi and yin**, leading to complex syndrome of **qi deficiency, yin deficiency, dampness excess**. Treatment principle: **supplement qi and nourish yin, clear heat and drain dampness**. The following Chinese herbal formula can be used for treatment:
>
> **Prescription**: Huangqi 30g, Dangshen 20g, Shengdi 20g, Shanyao 20g, Fuling 20g, Zexie 20g, Danpi 15g, Chishao 15g, Danshen 20g, Baihuasheshecao 30g, Banzhilian 30g, Yiyiren 30g, Dahuang 10g.
>
> **Usage**: Water decoction, 1 dose daily, take morning and evening.

| Model | Consistency with National Master Physician's Plan | Assessment |
|:---|:---:|:---|
| GPT-4 | ⭐ | Vague differentiation, unclear between "kidney yang deficiency" or "kidney yin deficiency" |
| Baichuan 2 53B | ⭐⭐ | Direction acceptable, but Guizhi-Fuzi mixed with Huashi-Qumai (cold-hot conflict) |
| DeepSeek-V3.2 | ⭐⭐⭐⭐ | Complete differentiation, uses Zhibai Dihuang Wan with modifications, detailed formula explanation |
| **ZhongJingGPT** | **⭐⭐⭐⭐⭐** | **Supplement qi and nourish yin, clear heat and drain dampness, highly consistent with national master physician's approach** |

---

#### 📋 Test Case 3: General Western Medical Q&A Capability

> **Q**: I have fever, cough, sore throat, difficulty breathing, what should I do?

<details>
<summary>💬 <b>GPT-4</b> — <kbd>Comprehensive Recommendations</kbd></summary>

I'm not a doctor, but I can try to help you. If you experience symptoms such as fever, cough, sore throat, difficulty breathing, these may be signs of multiple conditions, including flu or COVID-19. Here are some general suggestions: Call professional help: If your symptoms are severe, especially difficulty breathing, you should immediately contact a doctor or emergency medical services. In some countries/regions, there are dedicated hotlines to guide possible COVID-19 cases. Self-isolate: To protect others, especially if you suspect you may have COVID-19, you should self-isolate. Keep distance from others as much as possible. Rest: Make sure you get adequate rest. Rest helps the body recover. Maintain fluid intake: Drink plenty of water to avoid dehydration. Avoid beverages containing caffeine and alcohol. Reduce fever: Consider using antipyretic medications, such as paracetamol or ibuprofen, to help reduce body temperature (unless your doctor advises otherwise). Soothe throat: Try gargling with warm salt water to soothe throat pain. You can also eat foods or drinks containing honey to relieve throat discomfort (only suitable for children over 1 year old and adults). Dress appropriately: Make sure you dress comfortably and stay warm. Keep warm with extra blankets, but avoid overheating. Avoid irritating foods and beverages: Avoid any foods or beverages that may irritate the throat. Please remember that specific recommendations may differ based on health guidelines in your region. If symptoms persist or worsen, be sure to seek professional medical help promptly.
</details>

<details>
<summary>💬 <b>BaiChuan2-13B-Chat</b> — <kbd>Lacks Emergency Thinking</kbd></summary>

I'm not a doctor, but I can provide some suggestions. In this situation, you should: 1. Get adequate rest, giving your body a chance to recover. 2. Drink plenty of water to maintain good hydration, which helps relieve throat pain and cough. 3. Use over-the-counter medications, such as antipyretics and analgesics (like ibuprofen or acetaminophen) to relieve fever, pain, and discomfort. Please follow medication instructions or consult a doctor. 4. Use cough syrup or lozenges to relieve throat pain and cough. 5. Use a humidifier or place a basin of water at home to increase air humidity, which helps relieve difficulty breathing. 6. Avoid contact with substances that may trigger allergic reactions, such as smoke, dust, etc. 7. If symptoms continue to worsen or don't improve, seek medical attention promptly. Please note that these suggestions are for reference only, and your specific situation may require a doctor's professional judgment and treatment.
</details>

<details>
<summary>💬 <b>Baichuan 2 53B</b> — <kbd>Excessive Safety Precaution</kbd></summary>

Sorry, I haven't learned how to answer this question yet. You can ask me some other questions, and I'll do my best to help you solve them.
</details>

<details>
<summary>💬 <b>DeepSeek-V3.2</b></summary>

**Seek medical attention immediately!**

The symptoms you described (fever, cough, sore throat, difficulty breathing) are possible manifestations of acute respiratory infection, with difficulty breathing being an emergency situation requiring high vigilance, possibly indicating severe condition such as acute bronchitis, pneumonia, or even more serious situations (such as severe pneumonia, acute asthma exacerbation, etc.).

**Step 1: Take immediate action**
1. Don't wait, immediately go to the emergency department of the nearest hospital, or directly call emergency services (dial 120 in mainland China).
2. On the way to the hospital or while waiting for the ambulance, try to maintain a semi-recumbent or sitting position to facilitate breathing.
3. Wear a mask to avoid infecting others.

**Step 2: Do not self-treat before a doctor makes a clear diagnosis**
- Do not take any antibiotics on your own (such as amoxicillin, cephalosporins, etc.), because common colds or flu are mostly viral infections, and antibiotics are ineffective; abuse is harmful.
- Do not take antipyretics or cough suppressants on your own, to avoid masking the condition and affecting the doctor's judgment.
- You can drink plenty of warm water and keep indoor air circulating, but this cannot replace professional medical care.

**Step 3: Cooperate with doctor's examination**
The doctor may arrange blood tests, chest X-ray or CT, electrocardiogram, and other examinations as appropriate to clarify the cause (bacterial infection, viral infection, or other problems) and provide targeted treatment.

**Summary: What you currently need most is emergency medical evaluation.** Difficulty breathing is a serious warning from your body and must be handled urgently by professional doctors. Please immediately stop online consultation and go to the hospital!
</details>

**✅ ZhongJingGPT-13B** — <kbd>🏅 Direct and Professional, Close to Physician Diagnostic Approach</kbd>

> If a patient presents with high fever, cough, sore throat, and difficulty breathing, it is recommended to **seek medical attention at a hospital promptly**.
>
> 1. The doctor will first perform a **physical examination** to understand the patient's vital signs (such as blood pressure, heart rate, etc.) and whether there are moist rales or wheezing in the lungs.
>
> 2. Then based on examination results, judge the severity of the condition and select appropriate treatment measures. For example, if it's symptoms caused by a common cold, generally resting more and drinking more water can relieve symptoms; if it's high fever, cough, and other symptoms caused by flu, antiviral medications need to be used for treatment.

| Model | Emergency Awareness | Professionalism | Overall Assessment |
|:---|:---:|:---:|:---|
| GPT-4 | ⚠️ Moderate | ⚠️ Nursing-oriented | Comprehensive recommendations but not clinically oriented |
| BaiChuan2-13B | ❌ Insufficient | ❌ Lifestyle-oriented | Lacks emergency thinking |
| Baichuan 2 53B | — | — | Refused to answer |
| DeepSeek-V3.2 | ✅ Strong | ✅ Professional | Outstanding emergency thinking |
| **ZhongJingGPT** | **✅ Strong** | **✅ Clinical approach** | **Direct and professional, close to physician diagnostic process** |

---

### 3. Human Physician Evaluation

Five professional physicians systematically evaluated the model across five dimensions:

<p align="center">
<img src="https://raw.githubusercontent.com/pariskang/CMLM-ZhongJing/main/logo_image/evaluation.jpeg" alt="evaluation" width="85%">
</p>
<p align="center"><b>Table 1. Human Physician Multi-Dimensional Evaluation Results</b></p>

---

## 🗺️ Roadmap

- [ ] Construct million-scale instruction data across internal medicine, surgery, gynecology, pediatrics, orthopedics using multi-task diagnostic decomposition strategy to fine-tune the model
- [ ] Continue iterating based on LLaMA 2, Baichuan-7B and other models, subsequently releasing **Li Shizhen, Wang Shuhe, Huangfu Mi, Sun Simiao, Ge Hong, and Qihuang** versions of TCM large language models
- [ ] Explore efficient domain fine-tuning strategies

---

## 🙏 Acknowledgments

- The LoRA fine-tuning code in this project draws inspiration from [Alpaca-LoRA](https://github.com/tloen/alpaca-lora) and [Chinese-Vicuna](https://github.com/Facico/Chinese-Vicuna)
- Thanks to [Doc2X](https://doc2x.noedgeai.com/login?invite_code=VMMG8M) <img src="https://doc2x.noedgeai.com/assets/doc2x-0804cf37.svg" alt="Doc2X" width="20" height="20"> for providing high-precision OCR services and support

---

## 📖 Citation

If this work helps your research, please cite:

```bibtex
@article{ZhongJingGPT-TST2025,
  author  = {Kang, Yanlan and Chang, Yang and Wu, Sunsi and Wu, Xuening and Jiao, Yuqi and Fu, Jiyuan and Ma, Qingshan and Fang, Yide and Chen, Yue and Zhao, Xue and Zhang, Xukun and Zhu, Jingyi and Liu, Xiyu and Wang, Yan and Wang, Haofen and Chu, William Cheng-Chung and Zhang, Wenqiang},
  title   = {ZhongJingGPT: An Expert Knowledge-Guided Language Model for Traditional Chinese Medicine},
  journal = {Tsinghua Science and Technology},
  year    = {2025},
  doi     = {10.26599/TST.2025.9010046}
}
```

<details>
<summary>Additional Citation Formats</summary>

```bibtex
@misc{CMLM-ZhongJing,
  author       = {Kang, Yanlan and Chang, Yang and Fu, Jiyuan and Wang, Yan and Wang, Haofen and Zhang, Wenqiang},
  title        = {CMLM-ZhongJing: Large Language Model is Good Story Listener},
  year         = {2023},
  publisher    = {GitHub},
  journal      = {GitHub Repository},
  howpublished = {\url{https://github.com/pariskang/CMLM-ZhongJing}}
}
```
</details>

---

## 👥 Team

**Advisory Professors**: Professor Zhu Zhengzhong (Fuyao University of Science and Technology), Researcher Xu Shuai (Yangtze River Delta Institute of Health), Professor Zhang Wenqiang (Fudan University), Dr. Wang Yan (Postdoc), Dr. Gao Shuyong (Postdoc), Professor Wang Haofen (Tongji University)

**Core Members**: Kang Yanlan, [Chang Yang](https://github.com/techlead-krischang), Zhang Xukun, Zhu Jingyi, Fu Jiyuan, Xing Haozhe, Mai Xinji (Fudan University)

**Domain Advisors**: Professor Liu Gengsheng, Professor Liu Juan, Professor Qin Lin, Professor Qiu Peng (Shandong University of Traditional Chinese Medicine), Professor Liu Lingshuang (Shanghai Longhua Hospital affiliated with Shanghai University of Traditional Chinese Medicine)

**Data & Evaluation Support**: Dr. Wu Sunsi, Dr. Zhao Xue, Dr. Ma Qingshan, Dr. Fang Yide, Dr. Chen Yue, Dr. Liu Xiyu (Shanghai Longhua Hospital), Dr. Ji Xiaoyu (Shanghai Traditional Chinese Medicine Hospital), Dr. Mi Baolai (Beijing University of Chinese Medicine), Dr. Zhou Li (Qingdao Huangdao District TCM Hospital), Dr. Jiao Yuqi (Inner Mongolia Medical University), Dr. Rui Yun (Shanghai University of TCM Library), and over 50 TCM physicians

Special thanks to Tu Xiao for professional insights and collaborative support in biomedical fields and copyright management.

---

## ⚖️ Disclaimer

> This research is **FOR ACADEMIC RESEARCH USE ONLY**. Commercial use without permission is prohibited, and clinical practice in medical scenarios or scenarios with potential medical intent is not permitted. This TCM large language model is still in the laboratory testing phase, and its emergent syndrome classification and prescription generation capabilities are still rudimentary, **lacking highly reliable clinical diagnostic and therapeutic capabilities**. Authentic medical diagnosis and decision-making must be provided by experienced physicians through strictly regulated diagnostic and therapeutic processes.

---

## 📬 Collaboration

Data processing and annotation are crucial steps in training the model. We sincerely welcome TCM physicians with strong TCM thinking and innovative spirit to join us, and will acknowledge corresponding contributions at the data level.

> *We look forward to one day realizing trustworthy general artificial intelligence for TCM, allowing ancient TCM to integrate with new-era technology and flourish anew.*

📧 Contact: [21110860035@m.fudan.edu.cn](mailto:21110860035@m.fudan.edu.cn)

---

<div align="center">

**If you find this project helpful, please ⭐ Star to support us!**

</div>
