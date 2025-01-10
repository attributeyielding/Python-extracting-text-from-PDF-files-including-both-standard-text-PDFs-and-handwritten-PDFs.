Below is a Python script that uses PyPDF2, pdfplumber, and Tesseract OCR to process standard text-based PDFs and handwritten PDFs. The script extracts text from standard PDFs and uses Optical Character Recognition (OCR) for handwritten PDFs.
# Python Script: Reading PDFs (Text and Handwriting)
## Step 1: Install Required Libraries
Run the following to install the required dependencies:

```pip install PyPDF2 pdfplumber pytesseract opencv-python pillow pdf2image```
## Step 2: Install Tesseract OCR
1. Download and install Tesseract OCR.
2. Add Tesseract to your system's PATH.
## Step 3: Full Script

```
import os
from PyPDF2 import PdfReader
import pdfplumber
from pdf2image import convert_from_path
import pytesseract
import cv2

# Configure Tesseract OCR Path
pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'  # Update this path as needed

def extract_text_from_standard_pdf(file_path):
    """
    Extract text from standard PDFs using PyPDF2 and pdfplumber.
    """
    text = ""
    try:
        # Extract text using PyPDF2
        reader = PdfReader(file_path)
        for page in reader.pages:
            text += page.extract_text()
    except Exception as e:
        print(f"PyPDF2 failed with error: {e}. Trying pdfplumber...")

    # Fallback to pdfplumber
    if not text.strip():
        try:
            with pdfplumber.open(file_path) as pdf:
                for page in pdf.pages:
                    text += page.extract_text()
        except Exception as e:
            print(f"pdfplumber also failed with error: {e}")
            return None

    return text

def extract_text_from_handwritten_pdf(file_path, output_folder="output_images"):
    """
    Extract text from handwritten PDFs using OCR (Tesseract).
    """
    os.makedirs(output_folder, exist_ok=True)
    text = ""

    try:
        # Convert PDF pages to images
        images = convert_from_path(file_path)
        for i, image in enumerate(images):
            image_path = os.path.join(output_folder, f"page_{i + 1}.jpg")
            image.save(image_path, "JPEG")

            # Process the image for OCR
            img = cv2.imread(image_path)
            gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
            processed_img = cv2.adaptiveThreshold(
                gray, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 11, 2
            )
            # Perform OCR
            ocr_result = pytesseract.image_to_string(processed_img)
            text += f"Page {i + 1}:\n{ocr_result}\n\n"
    except Exception as e:
        print(f"Failed to process handwritten PDF: {e}")
        return None

    return text

if __name__ == "__main__":
    # Input: PDF file path
    pdf_file = input("Enter the path to your PDF file: ").strip()

    if not os.path.exists(pdf_file):
        print(f"File not found: {pdf_file}")
    else:
        pdf_type = input("Is the PDF standard or handwritten? (standard/handwritten): ").strip().lower()

        if pdf_type == "standard":
            extracted_text = extract_text_from_standard_pdf(pdf_file)
            if extracted_text:
                print("\nExtracted Text from Standard PDF:")
                print(extracted_text)
            else:
                print("No text found in the PDF.")
        elif pdf_type == "handwritten":
            extracted_text = extract_text_from_handwritten_pdf(pdf_file)
            if extracted_text:
                print("\nExtracted Text from Handwritten PDF:")
                print(extracted_text)
            else:
                print("No text found in the handwritten PDF.")
        else:
            print("Invalid input. Please specify 'standard' or 'handwritten'.")
```

## How It Works
1. ### Standard PDFs:

The script tries to extract text using PyPDF2.
If PyPDF2 fails, it uses pdfplumber as a fallback.

2. ### Handwritten PDFs:

Converts each PDF page to an image using pdf2image.
Preprocesses images (grayscale and thresholding) using OpenCV for better OCR accuracy.
Uses Tesseract OCR to extract handwritten text.

3. ### Output:

Extracted text is displayed in the console.
Images from handwritten PDFs are saved in the output_images directory for reference.


# Next Steps
## Enhancements:

1. Use EasyOCR for better handwriting recognition.
2. Add support for saving extracted text to files (.txt, .json).
3. Multi-language OCR:
4. Configure Tesseract to recognize text in different languages.

   
# Let me know if you need help extending or customizing this script! 
