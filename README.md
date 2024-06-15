# ImageRecognition
Image Recognition Program to Determine Pearlite and Ferrite Phases in Steel Material Microstructure

# Install the package
    pip install opencv-python-headless matplotlib

    pip install opencv-python numpy

    pip install matplotlib

# Create the model
import cv2
import numpy as np
import matplotlib.pyplot as plt

def calculate_composition(image):
    # Properly threshold the image
    # If ferrite is lighter, we use a direct threshold
    _, ferrite = cv2.threshold(image, 127, 255, cv2.THRESH_BINARY)
    # For pearlite (darker), we use an inverse threshold
    _, pearlite = cv2.threshold(image, 127, 255, cv2.THRESH_BINARY_INV)

    # Calculate total number of pixels
    total_pixels = image.size

    # Count the number of pixels in ferrite and pearlite regions
    ferrite_pixels = cv2.countNonZero(ferrite)
    pearlite_pixels = cv2.countNonZero(pearlite)

    # Calculate percentages
    ferrite_percentage = (ferrite_pixels / total_pixels) * 100
    pearlite_percentage = (pearlite_pixels / total_pixels) * 100

    return ferrite_percentage, pearlite_percentage, ferrite, pearlite

# Load the image in grayscale
image_path = 'Your Image File'  # Replace with the path to your image
image = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)

# Get the composition percentages and image masks
ferrite_percentage, pearlite_percentage, ferrite, pearlite = calculate_composition(image)

# Print the percentages
print(f"Ferrite: {ferrite_percentage:.2f}%")
print(f"Pearlite: {pearlite_percentage:.2f}%")

# Visualize the original image and the results
plt.figure(figsize=(12, 6))  # Adjust the size as needed
plt.subplot(1, 3, 1)  # 1 row, 3 columns, 1st subplot
plt.imshow(image, cmap='gray')
plt.title('Original Image')
plt.axis('off')

plt.subplot(1, 3, 2)  # 1 row, 3 columns, 2nd subplot
plt.imshow(ferrite, cmap='gray')
plt.title('Ferrite')
plt.axis('off')

plt.subplot(1, 3, 3)  # 1 row, 3 columns, 3rd subplot
plt.imshow(pearlite, cmap='gray')
plt.title('Pearlite')
plt.axis('off')

plt.tight_layout()
plt.show()
