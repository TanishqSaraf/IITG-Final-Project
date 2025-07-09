# Dynamic Pricing for Urban Parking Lots

![Parking Lot](https://img.shields.io/badge/Summer%20Analytics-2025-blue)

![Python](https://img.shields.io/badge/Python-3.8%2B-yellow)

![Pathway](https://img.shields.io/badge/Pathway-Streaming-orange)

![Bokeh](https://img.shields.io/badge/Bokeh-Visualization-green)## Project Overview

This repository contains the capstone project for **Summer Analytics 2025**, organized by the **Consulting & Analytics Club, IIT Guwahati**, in collaboration with **Pathway**. The project, "Dynamic Pricing for Urban Parking Lots," aims to optimize parking space utilization in urban areas by implementing real-time dynamic pricing. Three pricing models of increasing complexity are developed to adjust prices based on factors such as occupancy, queue length, traffic conditions, special events, and vehicle types.

### Objectives

- Develop three dynamic pricing models: Baseline Linear, Intermediate, and Advanced.
- Process streaming data in real-time using Pathway to simulate parking lot operations.
- Visualize price trends for multiple parking lots using Bokeh.
- Provide an explainable and scalable solution for urban parking management.
- Generate a report detailing model assumptions, demand functions, and justifications.

### Dataset

The dataset covers **73 days** with **18 time points per day** (every 30 minutes from 8:00 AM to 4:30 PM) for **14 parking lots**, totaling approximately 18,000 records. Key features include:

- `SystemCodeNumber`: Unique identifier for each parking lot.
- `Occupancy`: Number of occupied parking spaces.
- `Capacity`: Total parking spaces per lot.
- `QueueLength`: Number of vehicles waiting.
- `TrafficConditionNearby`: Traffic level ("low", "average", "high").
- `IsSpecialDay`: Indicator for special events (0 or 1).
- `VehicleType`: Type of vehicle ("car", "bike", "truck").
- `Timestamp`: Combined date and time of observation.

The sample dataset is available here.

## Pricing Models

### Model 1: Baseline Linear Model

- **Formula**: ( price_t = 0.9 \\cdot last_price + 0.1 \\cdot (10 + occupancy_rate_t) )
- **Features**: Occupancy rate (Occupancy / Capacity)
- **Description**: A simple model that adjusts prices based on occupancy, smoothed by the previous price, starting at a base price of $10.

### Model 2: Intermediate Model

- **Formula**: ( price_t = 0.9 \\cdot last_price + 0.1 \\cdot (10 + occupancy_rate_t + 0.1 \\cdot queue_length_t + traffic_code_t + is_special_day_t) )
- **Features**: Adds queue length, traffic condition (0, 1, 2), and special day indicator
- **Description**: Extends Model 1 by incorporating additional demand factors for more responsive pricing.

### Model 3: Advanced Model

- **Formula**: ( price_t = 0.9 \\cdot last_price + 0.1 \\cdot (10 + occupancy_rate_t + 0.1 \\cdot queue_length_t + traffic_code_t + is_special_day_t + vehicle_weight_t + 0.05 \\cdot occupancy_rate_t \\cdot queue_length_t) )
- **Features**: Adds vehicle type weights (0.5, 1.0, 1.5) and an interaction term
- **Description**: The most comprehensive model, balancing complexity and explainability by accounting for vehicle types and demand interactions.

## Repository Structure

```
├── dynamic_pricing_notebook.ipynb  # Main Jupyter notebook with models and visualizations
├── parking_stream.csv              # Sample streaming data (generated from dataset)
├── model1_prices.csv               # Output prices for Model 1
├── model2_prices.csv               # Output prices for Model 2
├── model3_prices.csv               # Output prices for Model 3
├── README.md                       # Project documentation
└── requirements.txt                # Python dependencies
```

## Prerequisites

- **Python**: 3.8 or higher
- **Google Colab**: Recommended for running the notebook with Google Drive integration
- **Dependencies**:

  ```plaintext
  pandas
  numpy
  matplotlib
  pathway
  bokeh
  panel
  ```

## Setup Instructions

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/your-username/dynamic-parking-pricing.git
   cd dynamic-parking-pricing
   ```

2. **Install Dependencies**: Create a virtual environment and install the required packages:

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. **Prepare the Dataset**:

   - Download the full dataset and place it in your Google Drive under `/Colab Notebooks/`.
   - Update the path in Cell 3 of the notebook to point to your dataset:

     ```python
     df = pd.read_csv('/content/drive/MyDrive/Colab Notebooks/your_dataset.csv')
     ```

4. **Mount Google Drive**: Run Cell 4 in the notebook to mount your Google Drive:

   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```

5. **Run the Notebook**:

   - Open `dynamic_pricing_notebook.ipynb` in Google Colab.
   - Execute the cells sequentially to:
     - Install dependencies (Cell 1).
     - Load and preprocess data (Cells 2–9).
     - Run the original sample pricing model (Cells 10–12).
     - Compute prices for the three models (Cells 13–15).
     - Visualize results and save outputs (Cells 16–17).
     - View the model summary (Cell 18).

## Usage

1. **Running the Models**:

   - The notebook processes streaming data using Pathway to simulate real-time pricing.
   - Each model (Cells 13–15) generates prices for each parking lot every 30 minutes, visualized with Bokeh plots.
   - Outputs are saved as `model1_prices.csv`, `model2_prices.csv`, and `model3_prices.csv`.

2. **Visualizations**:

   - Interactive Bokeh plots show price trends for up to 5 parking lots, with a clickable legend to toggle visibility.
   - Example output:

     ![Bokeh Plot](https://via.placeholder.com/800x400.png?text=Sample+Bokeh+Plot)

3. **Customizing Models**:

   - Adjust weights in the pricing formulas (e.g., 0.9, 0.1) in Cells 13–15 to tune price sensitivity.
   - Add new features to the dataset and update the schema in Cell 7.

## Challenges and Solutions

- **Challenge**: Handling stateful price computations in Pathway for multiple lots.
  - **Solution**: Used 30-minute tumbling windows to aggregate features and compute prices, ensuring smooth transitions.
- **Challenge**: Visualizing multiple parking lots without clutter.
  - **Solution**: Limited plots to 5 lots with a clickable legend for interactive exploration.
- **Challenge**: Ensuring compatibility with Pathway’s streaming model.
  - **Solution**: Added debugging cells and error handling to verify data flow.

## Results

- **Model Performance**: All models produce smooth, explainable price variations starting at $10, with Model 3 being the most responsive to demand factors.
- **Output Files**: CSV files contain time-series prices for each lot, suitable for further analysis or reporting.
- **Visualizations**: Interactive plots provide insights into price dynamics across lots.

## Future Improvements

- Incorporate additional features (e.g., competitor prices) if available.
- Optimize model weights using historical data and machine learning.
- Implement rerouting suggestions for overburdened lots.
- Enhance visualizations with filters for specific lots or time periods.

## Contributing

Contributions are welcome! Please:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Acknowledgments

- **Consulting & Analytics Club, IIT Guwahati**: For organizing Summer Analytics 2025.
- **Pathway**: For providing the streaming data processing framework.
- **Summer Analytics Team**: For guidance and support.

## Contact

For questions or feedback, please contact:

- **Your Name**: stanishq23@iitk.ac.in
- **GitHub**: TanishqSaraf

---

*Last updated: July 9, 2025*