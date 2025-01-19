# Floor Plan Analysis with CubiCasa

An advanced floor plan analysis tool that uses deep learning to detect, analyze, and visualize architectural floor plans with detailed room metrics, closet detection, and compliance checking.

## Features

- **Room Detection**: Automatically identifies and classifies different types of rooms (bedrooms, bathrooms, kitchen, etc.)
- **Icon Detection**: Recognizes architectural icons like doors, windows, closets, and appliances
- **Automatic Scaling**: Calculates real-world dimensions using standard door sizes
- **Room Analysis**: 
  - Calculates room dimensions and areas
  - Checks compliance with standard room size requirements
  - Provides detailed metrics for each room
- **Closet Detection**:
  - Identifies closets within bedrooms
  - Maps closet locations and relationships to rooms
- **Visualization**:
  - Color-coded room segmentation
  - Detailed room labeling with dimensions
  - Icon overlay visualization
  - Analysis tables with metrics

## Requirements

```
tensorboardX
tqdm
lmdb
svgpathtools
torch
torchvision
scikit-image
matplotlib
numpy
opencv-python
Pillow
scipy
```


## Usage

### Basic Usage

```python
from floortrans.models import get_model
from floortrans.post_prosessing import split_prediction
import matplotlib.pyplot as plt

# Load model
model = get_model('hg_furukawa_original', 51)
model.load_state_dict(checkpoint['model_state'])

# Process image
predictions = get_predictions_with_tta(image_path, model)

# Visualize results
visualize_tta_predictions(predictions, room_classes, icon_classes)
```

### Room Analysis

```python
# Get room metrics with door-based scaling
door_scale = get_door_width_scale(polygon_results['pol_icon_seg'], icon_classes)
room_metrics = analyze_rooms_with_closets(
    pol_room_seg,
    pol_icon_seg,
    room_classes,
    icon_classes,
    door_scale
)
```

## Standard Room Sizes

The system checks rooms against these standard minimum sizes:

- Bathroom: 3.0 m²
- Bedroom: 7.0 m²
- Entry: 4.0 m²
- Kitchen: 4.2 m²
- Living Room: 13.5 m²
- Storage: 3.0 m²
- Garage: 18.0 m²

## Model Architecture

The model uses a hourglass network architecture modified for floor plan analysis:
- Input: RGB floor plan image
- Output: 44 channel prediction (21 heatmaps, 12 room types, 11 icon types)
- Test-time augmentation with multiple rotations for robust predictions

## Visualization Types

1. **Basic Room Segmentation**:
   - Color-coded room types
   - Room dimensions and areas
   - Status indicators

2. **Icon Overlay**:
   - Architectural symbols
   - Doors and windows
   - Appliances and fixtures

3. **Closet Analysis**:
   - Bedroom-closet relationships
   - Closet placement visualization
   - Combined metrics display



## Acknowledgments

- Based on the CubiCasa5k dataset
- Uses modified Hourglass Network architecture
- Inspired by various architectural analysis tools

