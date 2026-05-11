# Data Augmentation for Power Plant Security — AI Engineering Pipeline

## Project Objective

**CyberEye Solutions** aims to enhance its visual recognition system for power plants by addressing the scarcity of real-world operational data. This pipeline tackles the problem by generating synthetic data and evaluating its impact on classifier performance.

## Pipeline Overview

The project implements a multi-phase pipeline:

1.  **Image Captioning (BLIP)**: Generates textual descriptions of real images.
2.  **Text Augmentation (Template-based)**: Produces caption variants to diversify prompts.
3.  **Image Generation (Stable Diffusion 1.5)**: Synthesizes new images from augmented captions.
4.  **Classifier (EfficientNet-B0)**: Fine-tuned on reduced and augmented datasets.
5.  **Evaluation**: Quantitative comparison (Accuracy, Precision, Recall, F1) between baseline and augmented models.

### Dataset

**OxfordIIITPet** (PyTorch) — Consisting of ~7,000 images across 37 dog and cat breeds. This serves as a proxy dataset, where each "breed" represents a category of object/behavior to recognize in a power plant.

## Technologies Used

| Phase                   | Technology               | Purpose                                        |
|-------------------------|--------------------------|------------------------------------------------|
| Image Captioning        | **BLIP** (Salesforce)    | Generate textual descriptions of real images   |
| Text Augmentation       | **Template-based**       | Produce caption variants to diversify prompts  |
| Image Generation        | **Stable Diffusion 1.5** | Synthesize new images from augmented captions  |
| Classifier              | **EfficientNet-B0**      | Fine-tuning on reduced and augmented datasets  |
| Optimization            | **AdamW + Cosine LR**    | Stable convergence, reduced overfitting        |
| Performance Enhancement | **FP16, xformers, DPM-Solver++** | Faster inference, memory optimization (for SD) |

## Key Results

The integration of Stable Diffusion synthetic data led to measurable improvements across core metrics:

-   **Overall Accuracy**: Increased by **+0.55%**, reaching a final accuracy of **77.54%** on the test set.
-   **Macro F1-Score**: Improved by **+0.43%**.
-   **Class-Specific Improvements**: Significant gains were observed in challenging categories like *Abyssinian*, *American Pit Bull Terrier*, *American Bulldog*, and *Chihuahua*.
-   **Reduced False Negatives**: Higher recall leads to fewer undetected threats.
-   **Increased Robustness**: Synthetic data introduces stylistic variations, improving robustness to lighting/angle changes.
-   **Scalability**: New anomaly categories only require new prompts, eliminating manual data collection.

### Performance Comparison (Test Set)

| METRIC                      | BASELINE | AUGMENTED | DELTA     |
|-----------------------------|----------|-----------|-----------|
| Accuracy                    | 0.7700   | 0.7754    | +0.0055   |
| Precision  (macro)          | 0.7760   | 0.7840    | +0.0079   |
| Recall     (macro)          | 0.7691   | 0.7746    | +0.0055   |
| F1 Score   (macro)          | 0.7670   | 0.7713    | +0.0043   |

### Anomalies to Monitor

A sharp decline in *Russian Blue* recall suggests that synthetic samples for this class might be introducing noise or overlapping with other feline breeds, warranting further prompt refinement.

## Future Developments

-   **ControlNet**: To generate structured industrial scenes (panels, cables, operators).
-   **Object Detection (YOLOv8)**: To localize anomalies instead of just classification.
-   **Continual Learning**: To add new threats without forgetting existing ones.

