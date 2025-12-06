# Using TFLite Models for Multi-Class Image Classification on Android

## Introduction

This project demonstrates how to train a deep learning model in Python and deploy it on Android using TensorFlow Lite (TFLite) or MNN for multi-class image classification.

> **Note**: Most of the training code was generated with the help of AI assistants (Qwen and Gemini 3 Pro). Our team collected the dataset: the `dog` and `cat` images come from Kaggle, while other classes were gathered via image searches on Baidu and Bing.

> ⚠️ **GPU Training Required**: The training script uses GPU acceleration. Ensure you have CUDA and cuDNN properly installed for your GPU. If your system lacks GPU support, request CPU-compatible training scripts from an AI assistant.

---

## Training Environment Setup

- **Python version**: 3.8.20 (minimum required)
- It is highly recommended to create a dedicated virtual environment to avoid dependency conflicts.

After activating your environment, install dependencies:

```bash
pip install -r requirements.txt
```

### Dataset Download

Download the dataset from [this 123Pan link](https://www.123865.com/s/vcnRVv-Q0U1h?pwd=T5qo) (password: `T5qo`).  
Extract the contents into the `Python/` directory so that the folder structure includes `datasets/`.

---

## Project Structure & File Descriptions

- **`datasets/`** – Contains training and validation image data.
- **`change_file_name.py`** – Renames files in `datasets/train_1/xxx/` for consistency.
- **`check_img.py`** – Validates whether images in `datasets/train_1/xxx/` are readable and suitable for training.
- **`split_test_dataset.py`** – Splits a portion of data into a test set (required before evaluation).
- **`predict_test_model.py`** – Evaluates the trained model on the test set.
- **`train_main.py`** – Main training script.
- **`models/`** – Automatically created; stores saved `.tflite` or `.h5` models.
- **`labels/`** – Automatically created; saves class label mappings (e.g., `labels.txt`).

---

## Deploying on Android with TFLite or MNN

### Requirements

| Framework  | Android API Level | Java Version | Additional Notes                      |
| ---------- | ----------------- | ------------ | ------------------------------------- |
| **TFLite** | ≥ 34              | 1.8          | No NDK required                       |
| **MNN**    | ≥ 29              | 1.8          | Requires NDK (version `20.0.5594570`) |

### Steps to Integrate

1. **Create a New Project**  
   In Android Studio, choose **"No Activity"** when creating the project.

2. **Import Project Structure**  
   Copy the folder structure from either:
   - `MNNClassification/` (for MNN inference)
   - `MyApplication3/` (for TFLite inference)

3. **Configure `build.gradle` (Module: app)**  
   - Replace `applicationId` with your actual package name.
   - Sync the project (**Sync Now**) to download dependencies before proceeding.

4. **Update `AndroidManifest.xml`**  
   Ensure the `package` attribute matches your application ID.

5. **Model Conversion (MNN Only)**  
   If using MNN, convert your trained `.tflite` model to `.mnn` format using the MNN converter tool:
   ```bash
   ./MNNConvert -f TFLITE --modelFile model.tflite --MNNModel model.mnn --bizCode biz
   ```

6. **Place Assets**  
   Put your model file (`.tflite` or `.mnn`) and `labels.txt` into the `app/src/main/assets/` directory.

---

## Final Notes

- The `MNNClassification` project was adapted from an open-source GitHub repository (author uncredited).
- Always verify image compatibility and label alignment before training.
- For CPU-only training, modify `train_main.py` to disable GPU usage (e.g., set `tf.config.set_visible_devices([], 'GPU')`).

With this setup, you can efficiently train custom image classifiers and deploy them on Android devices using lightweight inference engines like TensorFlow Lite or MNN.