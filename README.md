# Information Redaction System

## Description

An advanced AI-powered document redaction system that automatically detects and redacts Personally Identifiable Information (PII) and Protected Health Information (PHI) from PDF documents. The system uses LangChain with LLM integration to generate contextually appropriate dummy data replacements, ensuring documents remain useful while protecting sensitive information.

## Features

- **Intelligent PII/PHI Detection**:
  - Names, addresses, phone numbers, emails
  - Social Security Numbers (SSN)
  - Medical Record Numbers (MRN)
  - Health Plan Beneficiary Numbers
  - Medical conditions and medications
  - Doctor and hospital names
  - Credit card numbers, account numbers
  - IP addresses, URLs
  - Dates of birth and ages

- **Dual Redaction Modes**:
  - **Dummy Replacement**: LLM-generated realistic fake data
  - **Anonymization**: Generic placeholders (e.g., [NAME_1])

- **Visual Element Processing**:
  - Image classification and redaction
  - Text box overlays for images
  - Replacement image insertion
  - Maintains document layout

- **Document Processing**:
  - Docling-powered PDF parsing
  - OCR support for scanned documents
  - Page-by-page processing for accuracy
  - Coordinate-precise redaction

- **Policy Management**:
  - Configurable redaction policies
  - Global and entity-specific rules
  - YAML-based policy definitions
  - Audit logging

- **Output Options**:
  - Redacted PDF generation
  - Overlay PDF showing redaction locations
  - Color-coded redaction types
  - Processing logs and metrics

## Technologies Used

- **FastAPI** - Web framework for API endpoints
- **LangChain** - LLM orchestration and prompt management
- **OpenRouter** - LLM API integration
- **Docling** - Advanced document processing
- **PyMuPDF (fitz)** - PDF manipulation
- **Pydantic** - Data validation
- **OpenCV** - Image processing
- **BeautifulSoup** - Web scraping for replacement images
- **Python 3.9+**

## Architecture

```
Input PDF
    ↓
Document Enhancement (OCR if needed)
    ↓
Docling Parsing (Structure extraction)
    ↓
PII/PHI Detection (Page-by-page)
    ↓
LLM Dummy Data Generation
    ↓
Text Redaction (Coordinate-based)
    ↓
Image Classification & Redaction
    ↓
Overlay PDF Creation
    ↓
Final Redacted PDF
```

## Getting Started

### Prerequisites

- Python 3.9+
- OpenRouter API key

### Installation

```bash
# Install dependencies
pip install -r requirements.txt
```

### Configuration

Create a `.env` file:

```bash
OPENROUTER_API_KEY=your_openrouter_api_key
```

### Running the Application

```bash
# Start the FastAPI server
uvicorn app_main:app --host 0.0.0.0 --port 8000 --reload
```

## Usage

### Basic Redaction

```python
from app_main import SecureInfoRedactionPipeline

# Initialize pipeline
pipeline = SecureInfoRedactionPipeline("input.pdf")

# Process document
redacted_pdf = pipeline.process()
```

### Custom Policy

```yaml
# policy.yaml
global_settings:
  text_redaction_mode: 'dummy_replacement'  # or 'anonymize'
  visual_redaction_mode: 'text_box'  # or 'image'
  audit_logging: true
  create_overlay_pdf: true
```

```python
from app_main import SecureRedactionPolicyManager, SecureInfoRedactionPipeline

# Load custom policy
policy_manager = SecureRedactionPolicyManager("policy.yaml")
pipeline = SecureInfoRedactionPipeline("input.pdf", policy_manager)
```

## Redaction Modes

### Dummy Replacement Mode
- Uses LLM to generate realistic fake data
- Maintains document readability
- Contextually appropriate replacements
- Examples:
  - "John Doe" → "Michael Anderson"
  - "555-1234" → "(555) 987-6543"
  - "john@email.com" → "sarah.wilson@example.com"

### Anonymization Mode
- Generic placeholders with occurrence tracking
- Examples:
  - "John Doe" → "[NAME_1]"
  - "555-1234" → "[PHONE_1]"
  - "john@email.com" → "[EMAIL_1]"

## Visual Redaction

### Text Box Mode
- Overlays text boxes on images
- Displays classification labels
- Maintains document structure

### Image Replacement Mode
- Downloads contextually similar images
- Replaces sensitive visual content
- Preserves aspect ratios

## API Endpoints

```python
# Example FastAPI endpoints (if app_main.py is extended)
POST /redact - Redact a PDF document
POST /redact/batch - Batch redaction
GET /policies - List available policies
POST /policies - Create custom policy
```

## Processing Pipeline Details

### 1. Document Enhancement
- OCR for scanned documents
- Image quality improvement
- Text extraction optimization

### 2. PII/PHI Detection
- Page-by-page analysis
- Pattern matching with regex
- LLM-powered entity recognition
- Coordinate mapping to PDF

### 3. Dummy Data Generation
- LLM-based generation
- Caching for consistency
- Format preservation
- Contextual appropriateness

### 4. Redaction Application
- Coordinate-precise placement
- Font matching
- Color preservation
- Layout maintenance

### 5. Image Processing
- Classification with ML models
- Sensitive content detection
- Replacement or overlay
- Aspect ratio preservation

## Output Files

- **Redacted PDF**: Final document with all redactions applied
- **Overlay PDF**: Visualization of redaction locations
  - Green highlights: Dummy replacements
  - Red highlights: Anonymized content
- **Processing Log**: JSON log of all redactions and metrics

## Security Features

- **Memory-Only Processing**: No intermediate file storage
- **Secure Cleanup**: Automatic temporary file deletion
- **Audit Logging**: Complete redaction history
- **Policy Enforcement**: Configurable security rules
- **Hash Verification**: Integrity checking

## Performance Optimizations

- Page-by-page processing for large documents
- LLM response caching
- Parallel image processing
- Efficient coordinate mapping
- Garbage collection management

## Supported Document Types

- PDF (text and scanned)
- Images embedded in PDFs
- Multi-page documents
- Mixed content documents

## Limitations

- Handwritten text may require OCR tuning
- Complex layouts may need manual review
- Image redaction depends on classification accuracy
- LLM availability affects dummy data generation

## License

This project is licensed under the terms specified in the LICENSE file.

## Future Enhancements

- Support for DOCX, XLSX formats
- Real-time processing API
- Web interface for document upload
- Batch processing optimization
- Custom ML models for entity recognition
- Multi-language support
