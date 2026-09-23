# Final-Deep-Learning-Project-Semester-4


## Notebook 1 — Stock prediction
### 1) Dataset description
- Dataset: historical stock price data for AAPL and AMD from CSV files.
- Features: Date, Open, High, Low, Close, Volume.
- Objective: forecast future closing price from past price history.

Split strategy:
- Train/validation split using time order, with the last 1 year reserved as the test set.
- Model uses a 30-day sequence lookback.

### 2). Modelling
LSTM forecasting
- Uses an LSTM time-series model to predict the next price from a sequence of prior prices.
- Loss: MSE
- Evaluation metrics: MSE, MAE, MAPE, RMSE

Model comparison:
- baseline LSTM
- modified LSTM
- both trained on AAPL and AMD separately

### 3). Main Limitation
- It relies only on historical price data; macroeconomic factors, news sentiment, and volatility events are not included.
- Stock data is noisy and non-stationary, so prediction is inherently difficult.
- A single stock model may not generalize across different market regimes.

### 4). Final Reported Metric:
- AAPL baseline MSE ≈ 36.42, MAE ≈ 3.86; 
- AMD baseline MSE ≈ 1.10, MAE ≈ 0.94

Error is low for AMD and moderate for AAPL, but the model clearly benefits from the modified LSTM setup and better loss trends.

---
## Notebook 2 — Autoencoder reconstruction

### 1). Dataset Description

overhead images of two classes:
- Storage tank
- Parking lot

Preprocessing:
- grayscale conversion
- resize to 28 × 28
- normalize to [0, 1]
- combined into one dataset

Split:
- 80% train, 10% validation, 10% test
- stratified split to preserve class balance

### 2). Modelling

Encoder-decoder architecture:
    - encoder compresses image into latent representation
    - decoder reconstructs it back to 28 × 28
  
Baseline version:
    - standard conv + pooling + dense bottleneck
    - trained with MSE

Improved version:
    - deeper encoder and decoder
    - batch normalization
    - LeakyReLU
    - hybrid loss combining MSE and SSIM to reduce blur and better preserve structural detail

### 3). Main Limitation:

The dataset is relatively small for a deep autoencoder.
  - MSE alone makes reconstructions blurry.
  - SSIM improved the result, but validation performance still plateaued, indicating limited generalization.
  - 28 × 28 images are quite low-resolution for detailed spatial reconstruction.

### 4). Final metric report
- Final test SSIM improved from about 0.49 to about 0.60

Roughly a 22% increase; hybrid MSE + SSIM helped preserve structure better than plain MSE alone.

---
## Notebook 3 — Conditional GAN

### 1). Dataset Description
- Same overhead image dataset as project notebook 2 but used for generative modeling.
The generator and discriminator are conditioned on class labels:
- 0 = storage tank
- 1 = parking lot

Goal: generate synthetic overhead images resembling the real dataset.

### 2). Modelling
Conditional GAN architecture:
- generator receives random noise + class label
- discriminator receives image + class label
- Loss: binary cross entropy
- Evaluation metric: FID (Fréchet Inception Distance)
  
The model(s) also use checkpointing to save the best model based on lowest FID.

### 3). Main limitation
- GANs are unstable and sensitive to hyperparameters.
- The small image size and limited data make fine spatial details hard to reproduce.
- FID is useful, but it does not always align with human-perceived visual quality.

- Parking-lot patterns are especially difficult because they contain thin, repetitive, high-frequency structures.
- 
### 4). Final metric report
- FID values reported in roughly 126–151 range

Lower is better; the model generated visually plausible images, but the generator still struggled with detailed spatial patterns and was not fully sharp.
