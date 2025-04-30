

## 📊 Image Processing and Pattern Recognition (IPPR)

### 🔍 Key Topics to Focus On

#### 1. Color Image Fundamentals
- **RGB Color Model**
  - Understanding 24-bit color images (8 bits per channel)
  - RGB component separation and visualization
  - Channel operations and manipulations

- **CMY Color Model**
  - RGB to CMY conversion: `C = 255 - R, M = 255 - G, Y = 255 - B`
  - Applications of CMY in printing technologies
  - Channel separation and analysis

- **Other Color Models**
  - HSV, HSL, YCbCr transformations and applications
  - Color quantization techniques

#### 2. Image Domain Conversions
- Color to Grayscale conversion: `Gray = 0.299R + 0.587G + 0.114B`
- Binary image creation through thresholding
- Bit-plane slicing and analysis

#### 3. Image Enhancement Techniques
- **Histogram Operations**
  - Histogram calculation and visualization
  - Histogram equalization algorithm and implementation
  - Local vs. global histogram equalization
  - Effects on image contrast and brightness

- **Spatial Domain Enhancement**
  - Point processing operations
  - Brightness and contrast adjustment
  - Gamma correction

#### 4. Geometric Transformations
- **Basic Transformations**
  - Translation: `x' = x + tx, y' = y + ty`
  - Rotation: `x' = x·cos(θ) - y·sin(θ), y' = x·sin(θ) + y·cos(θ)`
  - Scaling: `x' = sx·x, y' = sy·y`
  - Reflection across axes
  - Shearing operations

- **Transformation Matrices**
  - Homogeneous coordinates
  - Combined transformation operations
  - First quadrant operations

#### 5. Frequency Domain Processing
- **Fourier Transform**
  - DFT/FFT implementation and interpretation
  - Magnitude and phase visualization
  - Inverse transforms and reconstruction

- **Cosine Transform**
  - DCT properties and applications
  - Relationship to image compression (JPEG)

- **Wavelet Transform**
  - Multi-resolution analysis
  - LL, LH, HL, HH subbands interpretation
  - Applications in compression and denoising
  - Image reconstruction from wavelet coefficients

#### 6. Image Filtering
- **Low-pass Filters**
  - Mean filter, Gaussian filter
  - Applications in noise reduction and smoothing

- **High-pass Filters**
  - Laplacian, unsharp masking
  - Applications in edge enhancement and sharpening

- **Image Sharpening Techniques**
  - Unsharp masking algorithm
  - High-boost filtering

#### 7. Segmentation Techniques
- **Edge Detection**
  - Sobel operator: horizontal and vertical kernels
  - Prewitt operator implementation
  - Canny edge detection algorithm
  - Performance comparison

- **Thresholding Methods**
  - Global thresholding
  - Otsu's method
  - Adaptive thresholding techniques

#### 8. Morphological Operations
- **Basic Operations**
  - Dilation: expanding objects
  - Erosion: shrinking objects
  - Structuring element design

- **Compound Operations**
  - Opening: erosion followed by dilation
  - Closing: dilation followed by erosion
  - Applications in noise removal and feature extraction

#### 9. Image Compression
- **Compression Fundamentals**
  - Lossless vs. lossy compression
  - Compression ratio calculation
  - Effect on aspect ratio

- **Compression Techniques**
  - Run-length encoding
  - Huffman coding
  - Transform-based compression

### 💻 IPPR Assignments
    available in /IPPR-ASSIGNMENTS folder (1-3)


### 🧪 Sample Code Snippets for IPPR Topics
    Concepts implementation available inside the ./IPPR-04/ Folder

#### Histogram Equalization
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Read image and convert to grayscale if needed
img = cv2.imread('image.jpg', 0)  # 0 for grayscale

# Calculate histogram
hist, bins = np.histogram(img.flatten(), 256, [0, 256])

# Calculate cumulative distribution function (CDF)
cdf = hist.cumsum()
cdf_normalized = cdf * hist.max() / cdf.max()

# Perform histogram equalization
img_equalized = cv2.equalizeHist(img)

# Display results
plt.figure(figsize=(12, 8))
plt.subplot(2, 2, 1)
plt.title('Original Image')
plt.imshow(img, cmap='gray')

plt.subplot(2, 2, 2)
plt.title('Equalized Image')
plt.imshow(img_equalized, cmap='gray')

plt.subplot(2, 2, 3)
plt.title('Original Histogram')
plt.plot(hist)
plt.xlim([0, 256])

plt.subplot(2, 2, 4)
plt.title('Equalized Histogram')
plt.plot(cv2.calcHist([img_equalized], [0], None, [256], [0, 256]).flatten())
plt.xlim([0, 256])

plt.tight_layout()
plt.show()
```

#### Geometric Transformations
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Read the image
img = cv2.imread('image.jpg')
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
rows, cols = img.shape[:2]

# Translation
tx, ty = 50, 30  # Translation in x and y direction
translation_matrix = np.float32([[1, 0, tx], [0, 1, ty]])
img_translated = cv2.warpAffine(img, translation_matrix, (cols, rows))

# Rotation
angle = 45  # Rotation angle in degrees
rotation_matrix = cv2.getRotationMatrix2D((cols/2, rows/2), angle, 1)
img_rotated = cv2.warpAffine(img, rotation_matrix, (cols, rows))

# Scaling
scale_x, scale_y = 0.5, 0.5  # Scaling factors
scaling_matrix = np.float32([[scale_x, 0, 0], [0, scale_y, 0]])
img_scaled = cv2.warpAffine(img, scaling_matrix, (cols, rows))

# Display results
plt.figure(figsize=(12, 8))
plt.subplot(2, 2, 1)
plt.title('Original Image')
plt.imshow(img)
plt.axis('off')

plt.subplot(2, 2, 2)
plt.title(f'Translated Image (tx={tx}, ty={ty})')
plt.imshow(img_translated)
plt.axis('off')

plt.subplot(2, 2, 3)
plt.title(f'Rotated Image ({angle} degrees)')
plt.imshow(img_rotated)
plt.axis('off')

plt.subplot(2, 2, 4)
plt.title(f'Scaled Image (sx={scale_x}, sy={scale_y})')
plt.imshow(img_scaled)
plt.axis('off')

plt.tight_layout()
plt.show()
```

#### Fourier Transform and Filtering
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Read image and convert to grayscale
img = cv2.imread('image.jpg', 0)

# Apply Fourier Transform
f = np.fft.fft2(img)
fshift = np.fft.fftshift(f)
magnitude_spectrum = 20 * np.log(np.abs(fshift) + 1)

# Create a high-pass filter (removing low frequencies)
rows, cols = img.shape
crow, ccol = rows // 2, cols // 2
mask = np.ones((rows, cols), np.uint8)
r = 30  # Filter radius
mask[crow-r:crow+r, ccol-r:ccol+r] = 0

# Apply mask and inverse FFT
fshift_filtered = fshift * mask
f_ishift = np.fft.ifftshift(fshift_filtered)
img_back = np.fft.ifft2(f_ishift)
img_filtered = np.abs(img_back)

# Display results
plt.figure(figsize=(12, 10))
plt.subplot(2, 2, 1)
plt.title('Original Image')
plt.imshow(img, cmap='gray')
plt.axis('off')

plt.subplot(2, 2, 2)
plt.title('Magnitude Spectrum')
plt.imshow(magnitude_spectrum, cmap='viridis')
plt.axis('off')

plt.subplot(2, 2, 3)
plt.title('High-Pass Filter Mask')
plt.imshow(mask, cmap='gray')
plt.axis('off')

plt.subplot(2, 2, 4)
plt.title('Filtered Image (High-Pass)')
plt.imshow(img_filtered, cmap='gray')
plt.axis('off')

plt.tight_layout()
plt.show()
```

#### Edge Detection Methods
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Read image and convert to grayscale
img = cv2.imread('image.jpg', 0)

# Apply different edge detection methods
edges_sobel_x = cv2.Sobel(img, cv2.CV_64F, 1, 0, ksize=3)
edges_sobel_y = cv2.Sobel(img, cv2.CV_64F, 0, 1, ksize=3)
edges_sobel = cv2.magnitude(edges_sobel_x, edges_sobel_y)
edges_sobel = cv2.normalize(edges_sobel, None, 0, 255, cv2.NORM_MINMAX).astype(np.uint8)

edges_prewitt_x = cv2.filter2D(img, -1, np.array([[-1, 0, 1], [-1, 0, 1], [-1, 0, 1]]))
edges_prewitt_y = cv2.filter2D(img, -1, np.array([[-1, -1, -1], [0, 0, 0], [1, 1, 1]]))
edges_prewitt = cv2.magnitude(np.float64(edges_prewitt_x), np.float64(edges_prewitt_y))
edges_prewitt = cv2.normalize(edges_prewitt, None, 0, 255, cv2.NORM_MINMAX).astype(np.uint8)

edges_canny = cv2.Canny(img, 100, 200)

# Display results
plt.figure(figsize=(12, 10))
plt.subplot(2, 2, 1)
plt.title('Original Image')
plt.imshow(img, cmap='gray')
plt.axis('off')

plt.subplot(2, 2, 2)
plt.title('Sobel Edge Detection')
plt.imshow(edges_sobel, cmap='gray')
plt.axis('off')

plt.subplot(2, 2, 3)
plt.title('Prewitt Edge Detection')
plt.imshow(edges_prewitt, cmap='gray')
plt.axis('off')

plt.subplot(2, 2, 4)
plt.title('Canny Edge Detection')
plt.imshow(edges_canny, cmap='gray')
plt.axis('off')

plt.tight_layout()
plt.show()
```

#### Morphological Operations
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Read image and convert to binary
img = cv2.imread('image.jpg', 0)
_, binary = cv2.threshold(img, 127, 255, cv2.THRESH_BINARY)

# Define kernel (structuring element)
kernel = np.ones((5, 5), np.uint8)

# Apply morphological operations
dilation = cv2.dilate(binary, kernel, iterations=1)
erosion = cv2.erode(binary, kernel, iterations=1)
opening = cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)  # Erosion followed by dilation
closing = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)  # Dilation followed by erosion

# Display results
plt.figure(figsize=(12, 10))
plt.subplot(2, 3, 1)
plt.title('Original Binary Image')
plt.imshow(binary, cmap='gray')
plt.axis('off')

plt.subplot(2, 3, 2)
plt.title('Dilation')
plt.imshow(dilation, cmap='gray')
plt.axis('off')

plt.subplot(2, 3, 3)
plt.title('Erosion')
plt.imshow(erosion, cmap='gray')
plt.axis('off')

plt.subplot(2, 3, 4)
plt.title('Opening (Erosion then Dilation)')
plt.imshow(opening, cmap='gray')
plt.axis('off')

plt.subplot(2, 3, 5)
plt.title('Closing (Dilation then Erosion)')
plt.imshow(closing, cmap='gray')
plt.axis('off')

plt.tight_layout()
plt.show()
```

---
