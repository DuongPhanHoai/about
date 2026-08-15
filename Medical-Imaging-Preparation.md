# Deep Learning Medical Imaging Preparation

## Purpose

Prepare for work on a deep-learning image-processing team supporting healthcare use cases such as human-organ detection or segmentation.

The central quality challenge is different from conventional software testing:

> Traditional software usually has deterministic expected results. Deep-learning systems are probabilistic, data-dependent, and evaluated through statistical and clinical evidence.

Use the checkboxes below to track progress. Confirm the team's actual product, imaging modality, model task, and technology stack before going deeply into any one topic.

---

## Step 1: Confirm the Team Context

- [ ] Confirm the primary imaging modality:
  - CT
  - MRI
  - Ultrasound
  - X-ray
  - Endoscopy or surgical video
  - Other
- [ ] Confirm the model task:
  - Image classification
  - Object or organ detection
  - Semantic segmentation
  - Instance segmentation
  - Image registration
  - Image enhancement or reconstruction
- [ ] Confirm whether processing is offline, interactive, or real-time.
- [ ] Confirm the main programming languages and frameworks.
- [ ] Confirm whether the team trains models, optimizes inference, develops SDKs, or performs all three.
- [ ] Confirm the target deployment environment: workstation, data center, cloud, edge device, or medical equipment.
- [ ] Confirm whether the product is research software, clinical decision support, or a regulated medical device.
- [ ] Ask what the team expects from an SDET during the initial onboarding period.

### Questions for the Manager

1. Which medical-image modalities and clinical workflows does the team support?
2. Is the primary task detection, segmentation, classification, or image reconstruction?
3. Which parts of the pipeline will I own or validate?
4. What are the principal quality problems today?
5. Which model-quality, performance, and release metrics are used?
6. Which public learning materials should I study before accessing internal documentation?

---

## Step 2: Learn Medical-Image Fundamentals

- [ ] Understand the differences among CT, MRI, ultrasound, X-ray, and endoscopy.
- [ ] Learn the basic structure of DICOM files.
- [ ] Learn the purpose of NIfTI files for three-dimensional medical images.
- [ ] Understand pixels, voxels, image spacing, slice thickness, and orientation.
- [ ] Understand image registration and coordinate systems.
- [ ] Learn CT Hounsfield Units and windowing.
- [ ] Understand image normalization, resizing, resampling, cropping, and padding.
- [ ] Understand 2D, 2.5D, and 3D model inputs.
- [ ] Learn how scanner type, acquisition protocol, and reconstruction method affect input data.

### Practical Exercise

- [ ] Load a public DICOM or NIfTI study.
- [ ] Display representative slices.
- [ ] Inspect dimensions, spacing, orientation, and metadata.
- [ ] Detect missing, duplicated, or incorrectly ordered slices.
- [ ] Apply normalization and resampling while verifying that anatomy and labels remain aligned.

---

## Step 3: Refresh Deep-Learning Fundamentals

- [ ] Review neural-network training, inference, loss, gradients, and optimization.
- [ ] Review convolutional neural networks.
- [ ] Understand encoder-decoder architectures and skip connections.
- [ ] Understand transfer learning and pretrained models.
- [ ] Understand overfitting, underfitting, regularization, and early stopping.
- [ ] Understand training, validation, and test dataset separation.
- [ ] Learn how class imbalance affects medical-image models.
- [ ] Understand model confidence and calibration.

### Relevant Model Families

- [ ] U-Net
- [ ] 3D U-Net
- [ ] nnU-Net
- [ ] Faster R-CNN
- [ ] YOLO
- [ ] Vision Transformer fundamentals
- [ ] Swin UNETR

The exact priority depends on the team's task. U-Net variants are especially relevant to organ segmentation, while detection models are more relevant when the required output is a bounding box.

### Relevant Loss Functions

- [ ] Cross-entropy loss
- [ ] Dice loss
- [ ] Focal loss
- [ ] Combined Dice and cross-entropy loss

Be able to explain why a model can achieve high pixel accuracy while failing to identify a small organ: background pixels dominate the image.

---

## Step 4: Learn Model-Evaluation Metrics

### Segmentation Metrics

- [ ] Dice score
- [ ] Intersection over Union
- [ ] Hausdorff distance
- [ ] Surface Dice
- [ ] Sensitivity and specificity per organ

### Detection Metrics

- [ ] Precision and recall
- [ ] Sensitivity
- [ ] False positives per image or study
- [ ] Intersection over Union for localization
- [ ] Mean average precision

### Evaluation Principles

- [ ] Calculate metrics per patient, organ, subgroup, and dataset—not only as one global average.
- [ ] Report metric distributions and worst cases, not only mean values.
- [ ] Understand the clinical cost of false positives and false negatives.
- [ ] Define thresholds with domain and clinical experts.
- [ ] Compare candidate models against an approved baseline.
- [ ] Maintain a representative golden dataset for regression testing.

### Important Interview and Work Question

> What is an acceptable failure threshold for each organ and clinical workflow, and what clinical or engineering evidence supports that threshold?

---

## Step 5: Build a Data-Quality Strategy

Many model failures originate from data rather than model code.

- [ ] Validate file format, schema, dimensions, orientation, and metadata.
- [ ] Detect corrupted files and missing or duplicated slices.
- [ ] Validate alignment between images and annotations.
- [ ] Check annotation completeness and inter-annotator disagreement.
- [ ] Identify duplicate studies and patients.
- [ ] Prevent patient-level leakage between training, validation, and test datasets.
- [ ] Measure class, organ, disease, demographic, and scanner imbalance.
- [ ] Check whether edge cases and rare conditions are represented.
- [ ] Compare distributions across hospitals, devices, and acquisition protocols.
- [ ] Detect protected patient information in images and metadata.
- [ ] Version datasets and transformations.
- [ ] Make preprocessing reproducible and auditable.

### Critical Rule

Split datasets by **patient**, not by individual image or slice. Otherwise, images from the same patient can appear in both training and evaluation data and produce misleading results.

---

## Step 6: Design Model and Pipeline Tests

### Data and Preprocessing

- [ ] Test input schemas and metadata constraints.
- [ ] Test preprocessing transformations independently.
- [ ] Verify that image and label transforms remain synchronized.
- [ ] Test empty, corrupted, incomplete, and unsupported inputs.
- [ ] Test unusual image sizes, spacing, orientation, and intensity ranges.

### Model Contracts

- [ ] Validate input and output dimensions and data types.
- [ ] Validate output ranges and class mappings.
- [ ] Validate model, configuration, and dataset version compatibility.
- [ ] Test model loading, initialization failure, and resource exhaustion.
- [ ] Verify deterministic behavior where deterministic execution is required.

### Model Regression

- [ ] Run each candidate model against the golden dataset.
- [ ] Compare accuracy and performance with the approved baseline.
- [ ] Define allowable regression thresholds per organ and subgroup.
- [ ] Save images, labels, predictions, overlays, metrics, and metadata for failed cases.
- [ ] Review newly introduced failures even when the overall average improves.

### Robustness

- [ ] Test image noise, blur, rotation, cropping, brightness, and contrast changes where clinically valid.
- [ ] Test missing slices or partially visible anatomy.
- [ ] Test different scanners and acquisition protocols.
- [ ] Test out-of-distribution and unsupported inputs.
- [ ] Test confidence calibration and uncertain predictions.
- [ ] Verify safe behavior when input quality is inadequate.

---

## Step 7: Learn the Relevant GPU and Python Stack

Confirm the actual team stack before treating every item as mandatory.

- [ ] Python
- [ ] NumPy
- [ ] OpenCV
- [ ] PyTorch
- [ ] pytest
- [ ] MONAI
- [ ] CUDA fundamentals
- [ ] cuDNN concepts
- [ ] TensorRT
- [ ] Triton Inference Server
- [ ] NGC containers
- [ ] Docker
- [ ] Nsight Systems
- [ ] Nsight Compute

### CUDA Concepts to Understand

- [ ] CPU versus GPU execution
- [ ] Parallel execution
- [ ] Threads, blocks, and grids
- [ ] Host and device memory
- [ ] Memory-transfer cost
- [ ] GPU synchronization
- [ ] Kernel execution
- [ ] Sources of nondeterminism

The initial goal is to understand the model execution and testing implications, not to become a CUDA-kernel specialist immediately.

---

## Step 8: Test Inference Performance

- [ ] Define representative input sizes and workloads.
- [ ] Separate model-loading time from steady-state inference time.
- [ ] Use warm-up runs before collecting latency measurements.
- [ ] Measure median and tail latency, not only average latency.
- [ ] Measure throughput and GPU memory consumption.
- [ ] Record GPU model, driver, CUDA, library, and model versions.
- [ ] Compare FP32, FP16, and INT8 accuracy and performance when applicable.
- [ ] Test single-input and batched inference.
- [ ] Test concurrent requests and long-duration execution.
- [ ] Detect memory leaks and resource exhaustion.
- [ ] Define performance-regression thresholds against an approved baseline.

### Benchmark Reproducibility

A performance report should contain enough environmental and configuration information for another engineer to reproduce the result.

---

## Step 9: Understand Healthcare Safety and Compliance

First confirm which standards apply to the team's product.

- [ ] Patient-data privacy and access control
- [ ] Data anonymization and retention
- [ ] Dataset, model, and software traceability
- [ ] Reproducible training and inference
- [ ] Model approval, versioning, and rollback
- [ ] Human review and override
- [ ] Risk analysis for false positives and false negatives
- [ ] Audit evidence
- [ ] ISO 14971 risk-management concepts
- [ ] IEC 62304 medical-software lifecycle concepts
- [ ] ISO 13485 quality-management concepts

Do not assume that every standard applies. Requirements differ among research tools, clinical decision-support software, and regulated medical devices.

---

## Step 10: Use AI Tools Safely

- [ ] Learn the employer's approved AI-tool policy.
- [ ] Do not submit internal code, medical data, model files, logs, or screenshots to unapproved services.
- [ ] Treat prompts and generated outputs as potentially confidential.
- [ ] Apply human review to AI-generated code and tests.
- [ ] Validate generated changes through deterministic checks.
- [ ] Maintain auditability for AI-assisted changes.
- [ ] Understand data-retention and model-training policies before using an external service.

Personal ChatGPT, Cursor, LM Studio, or other AI tools must not receive company-confidential information unless explicitly approved.

---

## Step 11: Complete a Public Medical-Imaging Exercise

Use a public dataset and an official or reputable MONAI example.

- [ ] Set up a reproducible Python environment.
- [ ] Load a public NIfTI or DICOM dataset.
- [ ] Inspect and validate image metadata.
- [ ] Visualize images and labels.
- [ ] Implement or reuse preprocessing transforms.
- [ ] Run a pretrained segmentation model or a small training experiment.
- [ ] Calculate Dice and Hausdorff metrics per patient and organ.
- [ ] Produce prediction overlays for failed cases.
- [ ] Compare model versions or configurations.
- [ ] Compare FP32 and FP16 inference if supported.
- [ ] Measure latency, throughput, and GPU memory.
- [ ] Create a concise quality report describing failures, limitations, and residual risks.

### Expected Deliverables

- Reproducible setup instructions
- Data-validation checks
- Automated model-evaluation script
- Per-patient and per-organ results
- Failed-case visualizations
- Accuracy and performance comparison
- Known limitations and recommended next actions

---

## Step 12: Prepare the Onboarding Approach

### Learn Before Changing

- [ ] Understand the product architecture and clinical workflow.
- [ ] Build and run the existing software.
- [ ] Execute the current test and evaluation suites.
- [ ] Study recent regressions, incidents, and difficult defects.
- [ ] Understand ownership boundaries and team dependencies.
- [ ] Learn why existing engineering decisions were made.

### Establish Credibility

- [ ] Complete one small end-to-end fix or test improvement.
- [ ] Provide concise, evidence-based progress updates.
- [ ] Ask domain-specific questions rather than immediately proposing framework replacement.
- [ ] Separate confirmed facts from assumptions.
- [ ] Document reproducible findings.

### Improve with Evidence

- [ ] Identify one high-value quality problem.
- [ ] Establish its baseline impact.
- [ ] Propose a focused improvement.
- [ ] Validate the improvement with measurable evidence.
- [ ] Document the approach so that other engineers can maintain it.

---

## Personal Priority Order

1. Confirm the team context and exact model task.
2. Learn the team's medical-image modality and data formats.
3. Refresh model architecture and evaluation fundamentals.
4. Focus deeply on data quality and patient-level evaluation.
5. Learn MONAI and the relevant GPU inference stack.
6. Build a small public-data validation project.
7. Study medical safety, privacy, and traceability requirements.
8. Enter the team with a learn-first, evidence-based improvement approach.

---

## Progress Notes

Use this section to record questions, findings, and links while following the plan.

### Team and Product

- Imaging modality:
- Model task:
- Clinical workflow:
- Deployment environment:
- Main languages and frameworks:
- My expected responsibilities:

### Quality Risks

- Highest-impact false negative:
- Highest-impact false positive:
- Important patient subgroups:
- Important scanner or hospital variations:
- Current evaluation metrics:
- Current performance requirements:

### Learning and Experiments

- Completed materials:
- Open questions:
- Exercise results:
- Topics requiring help from team specialists:
