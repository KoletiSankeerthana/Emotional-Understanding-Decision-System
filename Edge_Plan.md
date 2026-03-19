# EDGE / OFFLINE DEPLOYMENT PLAN

## Deployment Approach

The system is designed to run locally on-device (mobile or desktop) without relying on external APIs.

The pipeline includes:
- Text preprocessing (TF-IDF)
- Metadata processing
- XGBoost models for prediction
- Rule-based decision engine
- Uncertainty handling

Models and preprocessing objects can be saved using joblib and loaded during inference.

This enables:
- Offline usage
- Faster response time
- Better user privacy

---

## Running on Mobile / On-Device

To deploy on mobile:
- Convert models using ONNX or lightweight formats  
- Reduce feature size (TF-IDF features)  
- Use optimized inference libraries  

The decision engine is rule-based and lightweight, making it suitable for real-time execution.

---

## Model Size Considerations

Current setup:
- TF-IDF (~3000 features)
- XGBoost models

Optimization options:
- Reduce TF-IDF features (e.g., 3000 → 1000)
- Limit number of trees in XGBoost
- Replace XGBoost with Logistic Regression if needed

---

## Latency Considerations

The system is designed for low latency:
- TF-IDF transformation is fast for short text  
- XGBoost prediction is efficient  
- Decision logic is lightweight  

Expected latency:
- Few milliseconds per prediction  

Suitable for real-time applications.

---

## Trade-offs

Accuracy vs Model Size:
- Larger models improve accuracy but increase memory usage  

Latency vs Complexity:
- Complex models (like deep learning) improve understanding but increase latency  

Privacy vs Capability:
- On-device models ensure privacy but limit advanced processing  

---

## Final Design Choice

The system balances:
- performance  
- efficiency  
- interpretability  

by using:
- TF-IDF for lightweight text representation  
- XGBoost for prediction  
- rule-based logic for decisions  

This makes it suitable for real-world, on-device deployment.
