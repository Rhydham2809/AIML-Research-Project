Transfer Learning-Based Skin Lesion Classification Using DeiT-Small on the ISIC 2019 Dataset
Project overview
This project investigates deep-learning approaches for eight-class
dermoscopic skin-lesion classification using the ISIC 2019 dataset.
It compares a ResNet-50 baseline with DeiT-Small configurations and
reports the results of a later checkpoint-based fine-tuning run.

The final reported model is a fine-tuned DeiT-Small checkpoint. The
paper reports validation performance only; it does not claim independent
test-set or clinical performance.
Diagnostic categories
The classification task contains eight categories:

  Label   Diagnostic category
  MEL     Melanoma
NV      Melanocytic nevus
BCC     Basal cell carcinoma
AK      Actinic keratosis
BKL     Benign keratosis
DF      Dermatofibroma
VASC    Vascular lesion
SCC     Squamous cell carcinoma

The paper identifies 25,331 images in the ISIC 2019 training dataset.
Experiments reported
The paper compares four project configurations:
1. ResNet-50 + cross-entropy --- CNN baseline.
2. DeiT-Small + cross-entropy --- pretrained transformer with an
   eight-class classification head.
3. DeiT-Small + class-balanced focal loss --- effective-number
   class weighting, focal modulation, and label smoothing.
4. Fine-tuned DeiT-Small (final) --- continued fine-tuning from the
   selected DeiT-Small checkpoint. The paper states that the
   continuation script's default configuration selected the
   cross-entropy checkpoint for the final reported result.
Internal validation comparison
All scores below are reported in the paper as validation metrics.
  Model                  Accuracy       Balanced       Macro F1      Macro-AUC
configuration                         accuracy                
  ResNet-50 +              66.19%         47.96%         45.91%         90.97%
cross-entropy                                                 

  DeiT-Small +             72.17%         67.04%         65.48%         95.32%
cross-entropy                                                 

  DeiT-Small +             71.72%         70.61%         63.86%         94.35%
class-balanced
focal loss                                                    
Fine-tuned         86.19%     77.01%     79.85%     97.29%
DeiT-Small
(final)
The final model is reported on a stratified validation subset of 5,067
images. The corresponding split contains 20,264 training images and
5,067 validation images, using image-level stratified sampling, a
validation fraction of 0.20, and random seed 42.
Final model class-wise recall
  Class     Correct / total   Recall
  MEL             681 / 904   75.33%
NV          2,421 / 2,575   94.02%
BCC             571 / 665   85.86%
AK              114 / 173   65.90%
BKL             426 / 525   81.14%
DF                33 / 48   68.75%
VASC              42 / 51   82.35%
SCC              79 / 126   62.70%

The highest recall is reported for NV (94.02%). The lowest is reported
for SCC (62.70%). The paper also notes confusion between MEL and NV.
Method summary
Data partition and preprocessing
- Dataset: ISIC 2019 training dataset.
- Split: stratified, image-level 80:20 train-validation partition.
- Random seed: 42.
- Image format: RGB.
- Image size: 224 × 224 pixels.
- Training augmentation: random horizontal and vertical flips,
  rotations up to 15 degrees, and moderate color jitter.
- Normalization: ImageNet channel means (0.485, 0.456, 0.406) and
  standard deviations (0.229, 0.224, 0.225).
- Validation images receive deterministic resizing and normalization
  without random augmentation.
Models and losses
The paper describes ResNet-50 and DeiT-Small experiments. It includes
conventional cross-entropy and a class-balanced focal-loss experiment.
The focal-loss configuration uses effective-number class weights, focal
modulation, and label smoothing.

Important: The final reported fine-tuned DeiT-Small result is
associated with the cross-entropy checkpoint selected by the
continuation script's default configuration. The final score must not be
attributed to class-balanced focal loss.
Fine-tuning configuration
The paper reports the following continuation settings:
- Optimizer: AdamW.
- Learning rate: 2e-5.
- Weight decay: 1e-4.
- Batch size: 32.
- Maximum epochs: 10.
- Gradient clipping maximum norm: 1.0.
- Learning-rate scheduler: validation-metric-driven scheduler.
- Early stopping: patience of 3.
- Best checkpoint selection: validation macro F1-score.
Repository / file contents
The IEEE paper source is AIML Project Paper Final.tex.

The LaTeX source references the following figure file:
- confusion_matrix.png --- final model validation confusion matrix.
Keep the image in the same Overleaf project directory as the .tex
file, unless you update the image path in the LaTeX source.

This README describes the paper source and its reported experiments. It
does not imply that the training notebook, model checkpoints, ISIC
dataset, or complete training code are included in the same package. Add
those assets separately if you intend to distribute a fully reproducible
implementation.
Compile the IEEE paper
Option 1: Overleaf
1. Create or open an Overleaf project.
2. Upload AIML Project Paper Final.tex.
3. Upload confusion_matrix.png to the project root.
4. Set AIML Project Paper Final.tex as the main document.
5. Select pdfLaTeX as the compiler.
6. Compile the project.
The source uses the IEEE conference class:
\documentclass[conference]{IEEEtran}
It also uses the cite, amsmath, amssymb, graphicx, booktabs,
array, tikz, and url packages. Overleaf normally provides these
packages.
Option 2: Local LaTeX installation
With a LaTeX distribution such as TeX Live or MiKTeX installed, run:
pdflatex "AIML Project Paper Final.tex"
pdflatex "AIML Project Paper Final.tex"
Run the compiler twice so cross-references and citations can be
resolved. Ensure confusion_matrix.png is in the same directory.
Evaluation metrics
The paper reports four principal metrics:
- Accuracy: fraction of all validation predictions that are
  correct.
- Balanced accuracy: mean recall across the eight classes.
- Macro F1-score: unweighted mean of the class-wise F1-scores.
- Macro-AUC (OvR): mean one-versus-rest area under the ROC curve
  across classes.
The paper also reports a confusion matrix and class-wise recall.
Interpretation and limitations
- Results are validation results, not independent test-set results.
- The split is image-level, not patient-level or lesion-level.
- The cross-study comparison with the cited Ma et al. paper is
  contextual only. The validation subset sizes, architectures, and
  training protocols differ, so it is not a controlled head-to-head
  comparison.
- The focal-loss experiment improves balanced accuracy over the
  earlier DeiT-Small cross-entropy experiment, but does not improve
  all reported metrics.
- The final fine-tuned result is not evidence that focal loss caused
  the improvement.
- The paper does not establish external generalization, clinical
  readiness, or diagnostic reliability.
Suggested future work
The paper identifies the following directions:
1. Conduct controlled ablation experiments using an identical data
   split and training budget.
2. Repeat experiments with multiple random seeds.
3. Investigate patient-level or lesion-level partitioning where
   identifiers are available.
4. Evaluate on an independent external dataset.
5. Further analyze lesion-focused explanations.
Citation and dataset
The paper's bibliography contains the cited research on deep neural
networks for skin cancer classification, ResNet, Vision Transformers,
DeiT, focal loss, class-balanced loss, ISIC 2019, and related
dermoscopic classification work.

For the dataset, refer to the ISIC Challenge data page:

https://challenge.isic-archive.com/data/

For academic citation, use the references listed in the LaTeX paper and
verify their bibliographic details before publication or public release.
Authors listed in the paper
- Dr. Ankush Jain --- Netaji Subhas University of Technology (NSUT),
  New Delhi, India.
- Shamayita Biswas --- Netaji Subhas University of Technology (NSUT),
  New Delhi, India.
- Rhydham Gundigara --- Netaji Subhas University of Technology (NSUT),
  New Delhi, India.
