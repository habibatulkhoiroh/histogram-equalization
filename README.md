# histogram-equalization
```python
# Install OpenCV jika belum terpasang
!pip install opencv-python-headless

# Import pustaka yang dibutuhkan
import cv2
import matplotlib.pyplot as plt
import numpy as np
from google.colab import files

# Fungsi untuk menampilkan gambar
def show_image(image, title='Image', cmap_type='gray'):
    plt.imshow(image, cmap=cmap_type)
    plt.title(title)
    plt.axis('off')
    plt.show()

# Fungsi untuk menampilkan histogram
def show_histogram(image, title='Histogram'):
    plt.hist(image.ravel(), bins=256, range=[0, 256])
    plt.title(title)
    plt.show()

# Fungsi untuk memproses gambar seperti pada algoritma
def process_image(image):
    height, width = image.shape
    new_image = np.zeros_like(image)
    histogram = [0] * 256
    
    for i in range(height):
        for j in range(width):
            w = image[i, j]
            r = (w & 0xFF0000) >> 16
            g = (w & 0x00FF00) >> 8
            b = w & 0x0000FF
            xg = int((r + g + b) / 3)  # Konversi ke grayscale
            yg = int((256 * xg) / 128) // 128
            histogram[yg] += 1
            new_image[i, j] = yg
    
    return new_image, histogram

# Fungsi untuk histogram equalization
def histogram_equalization(image):
    equalized_image = cv2.equalizeHist(image)
    return equalized_image

# Upload gambar
uploaded = files.upload()

# Ambil nama file yang diunggah
for filename in uploaded.keys():
    print('Nama file yang diunggah:', filename)
    image_path = '/content/' + filename

# Baca gambar
original_image = cv2.imread(image_path, cv2.IMREAD_COLOR)
grayscale_image = cv2.cvtColor(original_image, cv2.COLOR_BGR2GRAY)

# Proses gambar sesuai algoritma
processed_image, histogram = process_image(grayscale_image)

# Tampilkan hasil
show_image(grayscale_image, title='Original Grayscale Image')
show_image(processed_image, title='Processed Image')

# Tampilkan histogram asli dan hasil proses
show_histogram(grayscale_image, title='Histogram of Original Grayscale Image')
plt.bar(range(256), histogram, color='gray')
plt.title('Histogram of Processed Image')
plt.show()

# Lakukan Histogram Equalization
equalized_image = histogram_equalization(processed_image)

# Tampilkan hasil Histogram Equalization
show_image(equalized_image, title='Equalized Image')
show_histogram(equalized_image, title='Histogram of Equalized Image')```
