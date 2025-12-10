# Advanced Shopify CSV Builder

A powerful, AI-enhanced web application built with Streamlit that transforms raw product data into Shopify-ready CSV imports with intelligent inventory management and real-time configuration updates.

## Features

### Core Functionality
- **Smart File Processing**: Supports CSV and Excel files with automatic column detection
- **AI-Powered Descriptions**: Three processing modes using Google's Gemini 2.5 Flash
- **Real-Time Configuration**: Instant preview updates as you change settings
- **Intelligent Inventory Management**: Extract quantities from size data or set manually
- **Size Sorting**: Automatic size ordering (XS, S, M, L, XL, etc.) with custom size support
- **Bulk Operations**: Apply quantities and prices across all variants simultaneously

### Advanced Features
- **Size-Based Surcharges**: Automatic price adjustments for larger sizes
- **Variant Generation**: Create all size/color combinations automatically
- **Data Validation**: Built-in checks for pricing issues and missing data
- **Spreadsheet-Style Editing**: Excel-like interface for bulk variant management
- **Progress Tracking**: Real-time processing feedback with progress bars

## Installation

### Prerequisites
- Python 3.8 or higher
- pip package manager

### Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd shopify-csv-builder
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Environment Configuration**
   Create a `.env` file in the root directory:
   ```env
   GEMINI_API_KEY=your_gemini_api_key_here
   ```

4. **Run the application**
   ```bash
   streamlit run app.py
   ```

The application will be available at `http://localhost:8501`

## Project Structure

```
shopify-csv-builder/
├── app.py                          # Main application entry point
├── requirements.txt                # Python dependencies
├── .env                           # Environment variables (not in repo)
├── backend/
│   ├── ai_service.py              # AI processing with Gemini
│   └── data_processor.py          # Core data transformation logic
├── frontend/
│   └── ui_components.py           # Streamlit UI components
├── helpers/
│   └── utils.py                   # Utility functions and helpers
└── config/
    ├── __init__.py               # Package initialization
    ├── constants.py              # Application constants
    └── settings.py               # User-configurable settings
```

## Usage Guide

### Step 1: File Upload
Upload your CSV or Excel file containing product data. The application automatically detects common column variations:

**Supported Columns** (case-insensitive):
- `title` / `Product Title` / `name` - Product name
- `description` / `Product Description` - Product description
- `colour` / `color` / `colors` - Colors (comma-separated)
- `product code` / `SKU` - Product SKU/code
- `size` / `sizes` - Sizes with quantities (format: `S-4,M-8,L-12`)
- `variant price` / `price` - Product price
- `variant compare at price` / `compare price` - Original/compare price
- And many more...

### Step 2: Configuration

Configure processing options in the sidebar:

**AI Processing Modes:**
- **Default template**: No AI processing
- **Simple mode**: Extract first sentence + generate tags
- **Full AI mode**: Complete description rewrite + tags

**Inventory Settings:**
- Set fallback quantities for variants
- Enable bulk quantity mode
- Configure inventory policies

**Pricing Options:**
- Set default compare prices
- Enable bulk compare price mode
- Configure size-based surcharges

### Step 3: Processing
Click "Start Processing" to:
- Parse and normalize your data
- Generate product variants (size/color combinations)
- Apply AI enhancements (if enabled)
- Create Shopify-compatible structure

### Step 4: Inventory Management
Fine-tune your variants:
- **Individual Management**: Edit quantities/prices per variant
- **Spreadsheet Editor**: Excel-like bulk editing interface
- **Bulk Operations**: Apply settings across all variants
- **Real-time Preview**: See changes immediately

### Step 5: Download
Review final statistics and download your Shopify-ready CSV file.

## Configuration Options

### AI Processing
The application supports three AI modes:

1. **Default Template**: Uses original descriptions without AI processing
2. **Simple Mode**: Extracts first sentence and generates relevant tags
3. **Full AI Mode**: Rewrites descriptions for better engagement and generates tags

### Size Surcharges
Configure automatic price increases for larger sizes:
```
Size: XXL, Surcharge: 10% = Original price × 1.10
Size: XXXL, Surcharge: 15% = Original price × 1.15
```

### Bulk Operations
- **Bulk Quantity**: Override all variant quantities with a single value
- **Bulk Compare Price**: Set uniform compare prices across all variants

## Input Data Format

### Size Format
Sizes should include quantities when available:
```
S-4,M-8,L-12,XL-16,XXL-20
```
This creates variants with extracted quantities:
- Size S: 4 units
- Size M: 8 units
- Size L: 12 units
- etc.

### Color Format
Colors should be comma-separated:
```
Red,Blue,Green,Black
```

### Example Input Row
```csv
title,description,colour,product code,size,variant price
"Cotton T-Shirt","Comfortable cotton t-shirt","Red,Blue,Black","CT001","S-10,M-15,L-12",299
```

## API Keys

### Google Gemini API
1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Add to your `.env` file as `GEMINI_API_KEY`

**Note**: AI features are optional. The application works without an API key in "Default template" mode.

## Dependencies

```
streamlit>=1.28.0
pandas>=1.5.0
numpy>=1.24.0
google-generativeai>=0.3.0
python-dotenv>=1.0.0
openpyxl>=3.1.0
```

## Error Handling

The application includes robust error handling for:
- **File Format Issues**: Automatic format detection and conversion
- **Missing Columns**: Graceful handling of optional columns
- **API Failures**: Fallback to original descriptions if AI processing fails
- **Data Validation**: Alerts for pricing issues and missing data
- **Memory Management**: Efficient processing of large files

## Performance Considerations

- **Batch Processing**: AI requests are batched with rate limiting
- **Memory Efficient**: Processes data in chunks for large files
- **Caching**: Session state caching for better performance
- **Real-time Updates**: Efficient diff-based UI updates

## Troubleshooting

### Common Issues

**"AI Features Disabled"**
- Check that `GEMINI_API_KEY` is set in your `.env` file
- Verify the API key is valid and has quota remaining

**"Could not load file"**
- Ensure file is in CSV or Excel format
- Check for special characters in column names
- Verify file is not corrupted

**"No variants generated"**
- Check that size and color columns contain valid data
- Ensure data uses comma-separated format
- Verify column names match expected patterns

**Preview not updating**
- Configuration changes should reflect immediately
- Try refreshing the page if issues persist
- Check browser console for JavaScript errors

### Data Validation Warnings

The application automatically detects:
- Missing or zero prices
- Empty required fields
- Invalid size formats
- Inconsistent data types

## Contributing

This application follows a modular architecture:

- **Backend**: Data processing and AI integration
- **Frontend**: UI components and user interaction
- **Config**: Application settings and constants
- **Helpers**: Utility functions and data manipulation

When contributing:
1. Maintain the existing code structure
2. Add appropriate error handling
3. Update configuration options in `settings.py`
4. Test with various input formats

## License

This project is intended for internal/commercial use. Please ensure compliance with:
- Google Gemini API Terms of Service
- Streamlit licensing requirements
- Any third-party library licenses

## Support

For technical issues or feature requests:
1. Check the troubleshooting section above
2. Review the application logs in the Streamlit interface
3. Verify your input data format matches the expected structure
4. Ensure all dependencies are correctly installed

---

**Built with**: Python 3.8+, Streamlit, Google Gemini AI, Pandas

**Last Updated**: 2025