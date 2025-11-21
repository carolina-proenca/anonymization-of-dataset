Project developed as part of the Security and Privacy course in the second semester of the 2024/2025 academic year.

---

## 1. Project Overview

This project focuses on the **anonymization of a dataset** using the ARX tool, with analysis of privacy risk and data utility. The dataset contains 9 attributes, including quasi-identifiers (QIDs) and sensitive attributes. The main goal is to reduce the re-identification risk while preserving the dataset's usefulness.

The work involves:  
- **Classification of attributes** into Identifying, Quasi-Identifiers, Sensitive, or Insensitive.  
- **Privacy risk analysis** before anonymization using ARX attacker models: Prosecutor, Journalist, and Marketer.  
- **Application of anonymization models**, including `k-anonymity + l-diversity` and `k-anonymity + t-closeness`.  
- **Evaluation of privacy and utility** using metrics such as Discernibility and Normalized-Unit Entropy.  
- **Analysis of parameter variations**, including `k`, `l`, `t`, suppression limits, and attribute weights.  

---

## 2. Dataset and Attribute Classification

- All attributes were initially classified as **quasi-identifiers** except for `salary-class`, which is **sensitive**.  
- Classification considered **distinction** (percentage of unique values) and **separation** (ability to separate sensitive attribute values).  
- Examples of combined QID effects on separation:  
  - `age + workclass → separation = 69.01%`  
  - `race + education → separation = 85.69%`  
  - `sex + race + native-country + workclass → separation = 79.66%`  

| Attribute         | Distinction (%) | Separation (%) |
|------------------|----------------|----------------|
| age              | 0.23           | 97.81          |
| occupation       | 0.046          | 89.46          |
| education        | 0.053          | 80.74          |
| marital-status    | 0.023          | 65.72          |
| sex              | 0.0066         | 43.82          |
| workclass        | 0.023          | 43.85          |
| race             | 0.016          | 25.10          |
| native-country   | 0.13           | 16.00          |

---

## 3. Privacy Risk Analysis (Before Anonymization)

ARX attacker models revealed high re-identification risk in the original dataset:  

- **Prosecutor Model:** 100% estimated risk, 72.86% records in risk, 46.49% at maximum risk.  
- **Journalist Model:** Similar to Prosecutor, but assumes less attacker knowledge.  
- **Marketer Model:** 60.04% estimated risk even with minimal attacker knowledge.  

These results highlight the necessity for anonymization to ensure privacy compliance.  

---

## 4. Applied Privacy Models

### 4.1 k-Anonymity + l-Diversity

- **k-anonymity** ensures each record is indistinguishable from at least `k-1` others.  
- **l-diversity** ensures sensitive attributes have at least `l` diverse values in each equivalence class.  
- Key observations:  
  - Fixing `L = 2`, increasing `k` reduces Estimated Journalist Risk significantly while maintaining high utility.  
  - Fixing `K = 5`, increasing `L` to 3 eliminates re-identification risk but drastically reduces utility (Discernibility → NaN, Normalized-Unit Entropy → 0%).  

### 4.2 k-Anonymity + t-Closeness

- **t-closeness** ensures the sensitive attribute distribution in each group is close to the global distribution (distance ≤ t).  
- Key observations:  
  - Fixing `t = 0.2`, increasing `k` lowers Estimated Journalist Risk with minimal utility loss.  
  - Fixing `k = 5`, increasing `t` improves data utility (Discernibility 40% → 85%, NU Entropy 8% → 21%) while maintaining low risk.  
- **Optimal configuration:** `K = 10, T = 0.2` balances risk and utility:  
  - Average Prosecutor Risk: 0.27431%  
  - Estimated Journalist/Prosecutor Risk: 10%  
  - Discernibility: 79.36%  
  - NU Entropy: 21.20%  

---

## 5. Parameter Variations

- **Suppression Limit:** Determines the maximum percentage of records that can be suppressed to meet privacy criteria.  
  - Lower limits preserve data but may prevent achieving anonymity.  
  - Higher limits increase privacy but reduce utility.  
- **Attribute Weighting (e.g., `age`):**  
  - Higher weight preserves important attributes during anonymization.  
  - Observed risk and utility stabilize beyond 20% weight for key attributes.  

---

## 6. Files Included

- `project.deid` – ARX project file with applied anonymization models.  
- `plots/` – Plots of privacy risk and utility metrics under varying parameters.  
- `analysis_document.pdf` – Detailed report of attribute classification, risk evaluation, and parameter analysis.  

---

## 7. Tools and References

- **ARX Data Anonymization Tool**: [https://arx.deidentifier.org](https://arx.deidentifier.org)  
- Key References:  
  - ARX Documentation and Privacy Models  
  - Data anonymization and risk/utility assessment literature  

---

## 8. Conclusion

This project demonstrates the application of anonymization techniques to reduce re-identification risk while preserving data utility. Both `k-anonymity + l-diversity` and `k-anonymity + t-closeness` effectively reduced privacy risk, with `K = 10, T = 0.2` providing the best trade-off between privacy and utility.
