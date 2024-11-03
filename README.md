# Extracting Account Information from PDFs using Azure OpenAI GPT-4  
   
This repository contains a Python script `analyze_images.py` that processes single-page PDF documents, converts them to images, and extracts specific account information using the Azure OpenAI GPT-4 model. The script saves both the generated images and the extracted information for further analysis or integration.  
   
## Table of Contents  
   
- [Features](#features)  
- [Prerequisites](#prerequisites)  
- [Setup](#setup)  
- [Usage](#usage)  
- [Example Output](#example-output)  
- [Directory Structure](#directory-structure)  
- [Customization](#customization)  
- [Notes](#notes)  
- [License](#license)  
   
## Features  
   
- **PDF to Image Conversion**: Converts the first page of each PDF into a high-quality PNG image using PyMuPDF.  
- **Information Extraction**: Utilizes Azure OpenAI GPT-4 to extract specified account information from the images.  
- **Saved Outputs**: Stores the generated images in `output_images` and the extracted data in `output_results`.  
- **Customizable**: Easily modify the prompt and output format to extract different information as needed.  
   
## Prerequisites  
   
- **Python 3.7** or higher  
- **Azure OpenAI Service** access with the GPT-4 model deployed  
- **Azure OpenAI Credentials**:  
  - Endpoint URL  
  - API Key  
  - Deployment Name  
- **Python Packages**:  
  - `openai`  
  - `pydantic`  
  - `python-dotenv`  
  - `PyMuPDF` (also known as `fitz`)  
- **Input Files**:  
  - Single-page PDF documents placed in the `input_documents` directory  
   
## Setup  
   
### 1. Clone the Repository  
   
```bash  
git clone https://github.com/yourusername/your-repo-name.git  
cd your-repo-name  
```  
   
### 2. Install Dependencies  
   
   
```bash  
pip install openai pydantic python-dotenv PyMuPDF  
```  
   
### 3. Configure Environment Variables  
   
Create a `.env` file in the root directory and add your Azure OpenAI credentials:  
   
```env  
AOAI_ENDPOINT=your_azure_openai_endpoint  
AOAI_API_KEY=your_azure_openai_api_key  
AOAI_DEPLOYMENT=your_azure_openai_deployment_name  
```  
   
Replace the placeholders with your actual Azure OpenAI endpoint, API key, and deployment name.  
   
### 4. Prepare Input PDFs  
   
- Create an `input_documents` directory in the root of the repository if it doesn't exist.  
- Place your single-page PDF documents into the `input_documents` folder.  
   
## Usage  
   
Run the script using the following command:  
   
```bash  
python analyze_images.py  
```  
   
The script will:  
   
1. Process each PDF in the `input_documents` directory.  
2. Convert the first page of each PDF into a high-resolution PNG image.  
3. Save the generated images into the `output_images` directory.  
4. Extract specified account information from each image using Azure OpenAI GPT-4.  
5. Save the extracted information as JSON files in the `output_results` directory.  
   

## Example Output  
   
After running the script, you should see output similar to:  
   
```bash  
Processing Contoso_Bank_example.pdf...  
Saved image to Contoso_Bank_example.png  
Saved extracted information to Contoso_Bank_example.json  
```  
   
The extracted information in `output_results/Contoso_Bank_example.json` might look like:  
   
```json  
{  
  "customerName": "John Doe",  
  "accountNumber": "1234",  
  "balanceUSD": "99.99"  
}  
```  
   
## Directory Structure  
   
```  
├── analyze_images.py  
├── .env  
├── input_documents/  
│   ├── sample1.pdf  
│   ├── sample2.pdf  
│   └── ... (your PDF files)  
├── output_images/  
│   ├── sample1.png  
│   ├── sample2.png  
│   └── ... (generated images)  
├── output_results/  
│   ├── sample1.json  
│   ├── sample2.json  
│   └── ... (extracted data)  
```  
   
- **`analyze_images.py`**: The main script that processes PDFs and extracts information.  
- **`.env`**: File containing your Azure OpenAI credentials.  
- **`input_documents/`**: Directory containing input PDF files.  
- **`output_images/`**: Directory where output images are saved.  
- **`output_results/`**: Directory where output JSON files are saved.  
   
## Customization  
   
You can adjust the script to suit your specific needs.  
   
### Adjusting the Zoom Factor  
   
The script uses a zoom factor to improve image quality:  
   
```python  
zoom = 2  # Zoom factor for image quality  
pix = page.get_pixmap(matrix=fitz.Matrix(zoom, zoom))  
```  
   
Increase the `zoom` variable for higher resolution images, if necessary.  
   
### Changing the Prompt and Output Fields  
   
Modify the `prompt` and `ExtractedAnswers` class to extract different fields:  
   
- **Modify the Prompt**: Change the `prompt` variable in the script to extract different information.   
  
  ```python  
  prompt = """Based on this image, answer the questions:  
              1) What is the invoice date?  
              2) List the items and their quantities."""  
  ```  
   
- **Update the Output Format**: Adjust the `ExtractedAnswers` class to match the information you're extracting.   
  
  ```python  
  class ExtractedAnswers(BaseModel):  
      invoiceDate: str  
      items: List[dict]  # Each item can be a dict with 'name' and 'quantity'  
  ```  

Another example

- **Modified Prompt**
  ```python  
  prompt = """Based on this image, answer the questions:  
              1) What is the invoice number?  
              2) What items were purchased?  
              3) What is the total amount due? Include currency."""  
  ```  
   
- **Modified Output Format**
  
  ```python  
  class ExtractedAnswers(BaseModel):  
      invoiceNumber: str  
      itemsPurchased: List[str]  
      totalAmountDue: str  
  ```  
   
## Notes  
   
- **Single-Page PDFs**: The script assumes each PDF contains only one page. If a PDF has multiple pages, only the first page will be processed.  
- **Error Handling**: The script includes basic error handling. Ensure your PDFs are not corrupt and are properly formatted.  
- **Azure OpenAI Access**: Make sure you have access to the GPT-4 model in your Azure subscription. Check your resource quotas and permissions.  
- **Python Environment**: It's recommended to use a virtual environment to manage your Python packages and dependencies.  
   
## License  
   
This project is licensed under the [MIT License](LICENSE).  
   