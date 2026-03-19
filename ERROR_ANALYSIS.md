# ERROR ANALYSIS

## Overview

I analyzed multiple failure cases from the validation set to understand where the model struggles. The errors mainly occur due to ambiguous language, short inputs, conflicting signals, and noisy labels.

---

## Failure Case 1 — Ambiguous Text

Text: "I'm fine, just tired"  
Actual: sad  
Predicted: neutral  

What went wrong: Mixed signals in text  
Why: Model treated "fine" as neutral  
Improvement: Use contextual embeddings or contradiction detection  

---

## Failure Case 2 — Short Input

Text: "ok"  
Actual: calm  
Predicted: anxious  

What went wrong: No emotional signal  
Why: Model forced a prediction with weak input  
Improvement: Use metadata fallback + mark uncertain  

---

## Failure Case 3 — Conflicting Signals

Text: "Feeling great but stressed"  
Actual: anxious  
Predicted: happy  

What went wrong: Positive + negative words together  
Why: Model focused on "great"  
Improvement: Detect contrast words like "but"  

---

## Failure Case 4 — Noisy Label

Text: "I had a normal day"  
Actual: anxious  
Predicted: calm  

What went wrong: Label mismatch  
Why: Dataset contains noisy labels  
Improvement: Data cleaning or label smoothing  

---

## Failure Case 5 — Weak Signal

Text: "Did some work today"  
Actual: neutral  
Predicted: happy  

What went wrong: No emotional words  
Why: Model inferred positivity from activity  
Improvement: Detect low-signal text  

---

## Failure Case 6 — Metadata Conflict

Text: "Feeling okay"  
Stress: high  
Actual: calm  
Predicted: anxious  

What went wrong: Metadata contradicted text  
Why: Model over-weighted stress  
Improvement: Balance feature influence  

---

## Failure Case 7 — Sarcasm

Text: "Everything is just perfect..."  
Actual: sad  
Predicted: happy  

What went wrong: Sarcasm missed  
Why: TF-IDF cannot detect tone  
Improvement: Use contextual models  

---

## Failure Case 8 — Multiple Emotions

Text: "Excited but nervous"  
Actual: anxious  
Predicted: happy  

What went wrong: Mixed emotions  
Why: Model picked dominant word  
Improvement: Multi-emotion handling  

---

## Failure Case 9 — Low Context

Text: "Not sure how I feel"  
Actual: anxious  
Predicted: neutral  

What went wrong: Ambiguous input  
Why: No strong features  
Improvement: Add context or history  

---

## Failure Case 10 — Fatigue Misinterpretation

Text: "Too tired to think"  
Actual: sad  
Predicted: calm  

What went wrong: Fatigue seen as calm  
Why: Lack of domain understanding  
Improvement: Add fatigue-specific rules  

---

## Key Insights

- Short and ambiguous text leads to unreliable predictions  
- Conflicting signals reduce model accuracy  
- Metadata helps but can also mislead  
- Noisy labels affect performance  
- TF-IDF struggles with sarcasm and context  

The system addresses these issues using uncertainty modeling and decision logic.
