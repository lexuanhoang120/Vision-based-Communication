# Vision-based Communication Project

## Description

This repo, Vision-based Communications with Object Tracking, implements a vision-guided beamforming pipeline for vehicular wireless links: YOLO detects vehicles, a Kalman filter refines their trajectories, and linear regression predicts future positions so the antenna can pre-steer, mitigating latency-induced misalignment, preserving gain, and boosting data rates; the repository includes reproducible highway and urban simulations benchmarking the predictive approach against a conventional baseline, complete with metrics, plots, and notebooks for rapid evaluation.

## Project Structure

```
Vision-based-Communication/
├── src/                          # Source code directory
│   ├── detection/                # Object detection modules
│   │   ├── __init__.py
│   │   ├── detector.py          # YOLO-based detection
│   │   └── config.py            # Detection configuration
│   ├── tracking/                 # Object tracking modules
│   │   ├── __init__.py
│   │   ├── tracker.py           # Multi-object tracking
│   │   └── kalman_filter.py     # Kalman filter implementation
│   ├── estimation/               # Position estimation modules
│   │   ├── __init__.py
│   │   ├── predictor.py         # Future position prediction
│   │   └── coordinate_mapping.py # 3D coordinate conversion
│   ├── evaluation/               # Evaluation and analysis modules
│   │   ├── __init__.py
│   │   ├── evaluator.py         # Performance evaluation
│   │   └── visualizer.py        # Visualization tools
│   └── utils/                    # Utility functions
│       ├── __init__.py
│       └── data_utils.py        # Data processing utilities
├── models/                       # Model weights and configurations
│   └── ultralytics/             # YOLO model files
├── datasets/                     # Dataset directory
├── results/                      # Output results and visualizations
├── scripts/                      # Execution scripts
├── requirements.txt              # Python dependencies
└── README.md                     # Project documentation
```

## Installation

### Prerequisites

- Python 3.8 or higher.
- CUDA-compatible GPU (recommended for optimal performance).
- Git.

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Vision-based-Communication
   ```

2. **Create a virtual environment (recommended)**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Download pre-trained models**
   The system will automatically download YOLO11n weights on first run, or you can manually download them to `models/ultralytics/weights/`.

## Usage

### 1. Object Detection

Run object detection on images or videos:

```bash
python src/detection/detector.py \
    --source_dir datasets/Data4Simulation \
    --conf_score 0.5 \
    --visualize \
    --save_results \
    --save_dir detection_results
```

### 2. Object Tracking

Track objects in video sequences:

```bash
python src/tracking/tracker.py \
    --input_type folder \
    --input_path datasets/Data4Simulation \
    --save_output \
    --output_file tracked_output.mp4 \
    --fps 10
```

### 3. Performance Evaluation

Evaluate tracking performance and communication rates:

```bash
python src/evaluation/evaluator.py \
    --input_detection_json detection_results.json \
    --window_size 5 \
    --gain_exp_scale 2
```

### 4. Visualization

Generate performance plots and 3D visualizations:

```bash
python src/evaluation/visualizer.py \
    --results_file estimation_results.json \
    --output_dir results/visualizations
```

## Configuration

The system uses configuration files for different components:

- **Detection Configuration**: `src/detection/config.py`
- **Tracking Configuration**: `src/tracking/config.py`

Key parameters include:
- Model paths and weights.
- Confidence thresholds.
- Tracking algorithms.
- Evaluation metrics.

## Data Format

### Input Data
- **Images**: Supported formats: JPG, PNG, BMP.
- **Videos**: Supported formats: MP4, AVI.
- **Detection Results**: JSON format with bounding box coordinates.

### Output Data
- **Tracking Results**: JSON format with trajectory data.
- **Evaluation Metrics**: Performance comparison between baseline and proposed methods.
- **Visualizations**: PNG plots and MP4 videos.
