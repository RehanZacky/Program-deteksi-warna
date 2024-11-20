# Program-deteksi-warna
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
from google.colab import files

def detect_banana_ripeness(image_path):
    # Membaca gambar
    image = cv2.imread(image_path)
    image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
    
    # Konversi ke HSV untuk analisis warna
    hsv_image = cv2.cvtColor(image, cv2.COLOR_RGB2HSV)
    
    # Masking warna kuning (untuk pisang matang)
    lower_yellow = np.array([20, 100, 100])  # Rentang bawah kuning
    upper_yellow = np.array([30, 255, 255])  # Rentang atas kuning
    yellow_mask = cv2.inRange(hsv_image, lower_yellow, upper_yellow)
    
    # Masking warna hijau (untuk pisang mentah)
    lower_green = np.array([35, 100, 100])  # Rentang bawah hijau
    upper_green = np.array([85, 255, 255])  # Rentang atas hijau
    green_mask = cv2.inRange(hsv_image, lower_green, upper_green)
    
    # Masking warna coklat (untuk pisang terlalu matang)
    lower_brown = np.array([10, 100, 20])  # Rentang bawah coklat
    upper_brown = np.array([20, 255, 200])  # Rentang atas coklat
    brown_mask = cv2.inRange(hsv_image, lower_brown, upper_brown)
    
    # Menghitung persentase masing-masing warna
    yellow_area = np.sum(yellow_mask > 0)
    green_area = np.sum(green_mask > 0)
    brown_area = np.sum(brown_mask > 0)
    total_area = image.shape[0] * image.shape[1]
    
    yellow_percentage = (yellow_area / total_area) * 100
    green_percentage = (green_area / total_area) * 100
    brown_percentage = (brown_area / total_area) * 100
    
    # Menentukan kematangan
    if yellow_percentage > green_percentage and yellow_percentage > brown_percentage:
        ripeness = "Matang"
    elif green_percentage > yellow_percentage and green_percentage > brown_percentage:
        ripeness = "Belum Matang"
    else:
        ripeness = "Terlalu Matang"
    
    # Menampilkan hasil
    print("Persentase Kuning (Matang): {:.2f}%".format(yellow_percentage))
    print("Persentase Hijau (Belum Matang): {:.2f}%".format(green_percentage))
    print("Persentase Coklat (Terlalu Matang): {:.2f}%".format(brown_percentage))
    print("Kesimpulan: Pisang Anda {}".format(ripeness))
    
    # Plot hasil
    plt.figure(figsize=(12, 6))
    plt.subplot(1, 2, 1)
    plt.title("Gambar Asli")
    plt.imshow(image)
    plt.axis('off')
    
    plt.subplot(1, 2, 2)
    plt.title("Mask Kuning")
    plt.imshow(yellow_mask, cmap='gray')
    plt.axis('off')
    plt.show()

# Fitur unggah gambar
print("Silakan unggah gambar pisang:")
uploaded = files.upload()

# Mengambil nama file yang diunggah
for image_name in uploaded.keys():
    print(f"Gambar yang diunggah: {image_name}")
    detect_banana_ripeness(image_name)

```
# Output
Pisang berwarna hijau :
![pisang hijau](https://github.com/user-attachments/assets/6b7f4832-56ef-43c3-b81e-8b1fba811c40)
Pisang berwarna kuning :
![pisang kuning](https://github.com/user-attachments/assets/84b9e7ed-a600-474a-8971-f16bcb593a44)

# Penjelasan
Jadi, program diatas digunakan untuk mendeteksi kematangan buah pisang menggunakan menggunakan sistem deteksi warna.
Dengan menggunakan masking warna kuning dan hijau saat program mendeteksi lebih banyak warna kuning program dapat mengetahui jika buah pisang sudah matang dan saat warna hijau lebih banyak maka program mengetahui jika buah pisang belum matang, dan jika warna coklat terdeteksi maka buah tersebut dikategorikan terlalu matang.
warna tersebut dilihat dari presentase warna yang terdeteksi.
Program diatas menggunakan bahasa pemrograman python.
