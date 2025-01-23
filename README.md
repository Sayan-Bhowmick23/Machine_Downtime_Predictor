# Predictive Analysis for Manufacturing Operations

## **Overview**
This project implements a RESTful API for predictive analysis in manufacturing operations. The API allows users to upload manufacturing data, train a machine learning model to predict machine downtime, and make predictions based on input data. The solution uses Python, Flask, and scikit-learn to create a lightweight and efficient system for predictive analytics.

---

## **Features**
- **Upload Endpoint (`POST /upload`)**: Accepts a CSV file containing manufacturing data.
- **Train Endpoint (`POST /train`)**: Trains a logistic regression model on the uploaded dataset and returns performance metrics (accuracy and F1-score).
- **Predict Endpoint (`POST /predict`)**: Accepts JSON input with feature values and returns predictions (e.g., whether there will be downtime or not) along with confidence scores.

---

## **Setup Instructions**

### **Prerequisites**
Ensure the following are installed:
1. Python 3.8 or higher
2. pip (Python package manager)

### **Installation**
1. Clone the repository:
   ```bash
   git clone <repository_url>
   cd <repository_directory>
   ```
2. Install required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### **Run the Application**
Start the Flask application by running:
```bash
python app.py
```
The API will be available at `http://127.0.0.1:5000`.

---

## **API Endpoints**

### **1. Upload Data**
**Endpoint**: `POST /upload`

**Description**: Upload a CSV file containing manufacturing data.

**Request**:
- Content-Type: `multipart/form-data`
- File format: `.csv`

Example cURL command:
```bash
curl -X POST -F "file=@data.csv" http://127.0.0.1:5000/upload
```

**Response**:
- Success:
  ```json
  {
    "message": "Data uploaded successfully",
    "shape": [100, 15]
  }
  ```
- Error:
  ```json
  {
    "error": "Invalid file format"
  }
  ```

---

### **2. Train Model**
**Endpoint**: `POST /train`

**Description**: Train a logistic regression model on the uploaded dataset.

**Request**: No additional parameters required.

Example cURL command:
```bash
curl -X POST http://127.0.0.1:5000/train
```

**Response**:
- Success:
  ```json
  {
    "message": "Model trained successfully",
    "accuracy": 0.85,
    "f1_score": 0.83
  }
  ```
- Error:
  ```json
  {
    "error": "No data uploaded"
  }
  ```

---

### **3. Predict**
**Endpoint**: `POST /predict`

**Description**: Make predictions using the trained model.

**Request**:
- Content-Type: `application/json`
- JSON body with feature values.

Example cURL command:
```bash
curl -X POST -H "Content-Type: application/json" \
-d '{
      "Date": "23-01-2025",
      "Machine_ID": "M001",
      "Assembly_Line_No": "A1",
      "Hydraulic_Pressure(bar)": 100,
      "Coolant_Pressure(bar)": 50,
      "Air_System_Pressure(bar)": 75,
      "Coolant_Temperature": 40,
      "Hydraulic_Oil_Temperature(?C)": 60,
      "Spindle_Bearing_Temperature(?C)": 70,
      "Spindle_Vibration(?m)": 0.02,
      "Tool_Vibration(?m)": 0.03,
      "Spindle_Speed(RPM)": 1200,
      "Voltage(volts)": 220,
      "Torque(Nm)": 15,
      "Cutting(kN)": 5
    }' \
http://127.0.0.1:5000/predict
```

**Response**:
- Success:
  ```json
  {
    "Downtime": "No_Machine_Failure",
    "Confidence": 0.92,
    "Prediction": 1
  }
  ```
- Error:
  ```json
  {
    "error": "Model not trained"
  }
  ```

---

## **Project Structure**
```
├── app.py                 # Main Flask application file
├── requirements.txt       # Python dependencies
├── README.md              # Project documentation
└── sample_data.csv        # Example dataset for testing (if applicable)
```

---

## **Sample Dataset**
The dataset should include the following columns:
- `Date` (e.g., `23-01-2025`)
- `Machine_ID` (e.g., `M001`)
- `Assembly_Line_No` (e.g., `A1`)
- Numerical features such as pressures, temperatures, vibrations, etc.
- Target column: `Downtime` (binary classification).

You can use synthetic or real manufacturing datasets from sources like Kaggle or UCI ML Repository.

---

## **Technical Details**
1. **Preprocessing Pipelines**:
   - Numerical features are scaled using `StandardScaler`.
   - Categorical features are encoded using `OneHotEncoder`.
   - Ordinal encoding is applied to day and month columns derived from dates.

2. **Model Training**:
   - Logistic Regression is used for prediction.
   - The preprocessor and model are saved using `joblib`.

3. **Error Handling**:
   - Input validation ensures required fields are present.
   - Proper error messages are returned for invalid requests.

---

## **Testing**
Use tools like Postman or cURL to test the endpoints locally.

---

## **Future Enhancements**
- Add support for additional machine learning models.
- Implement authentication for API endpoints.
- Deploy the API to cloud platforms like AWS or Azure.
  
