# Lane Detection

A Python-based computer vision project that detects road lane markings using edge detection and Hough Transform algorithms.

## Overview

This project implements a lane detection pipeline using OpenCV. It identifies lane markings on roads by:
- Preprocessing with median filtering
- Detecting edges using Canny edge detector
- Defining a Region of Interest (ROI) to focus on the road area
- Applying Hough Transform to detect straight lines
- Visualizing detected lanes on the original image

## Features

- **Canny Edge Detection**: Multi-stage algorithm for precise, thin edge detection
- **ROI Masking**: Trapezoidal region focus to eliminate irrelevant areas
- **Hough Transform**: Pattern recognition to detect straight lane lines
- **Adjustable Parameters**: Customizable thresholds and filters for different road conditions
- **Accumulator Visualization**: Visual representation of Hough voting process

## Requirements

See `requirements.txt` for all dependencies. Main libraries:
- OpenCV (cv2)
- NumPy
- Matplotlib

## Installation

1. Clone this repository:
```bash
git clone <your-repo-url>
cd lane-detection
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage

1. Place your road images in the `images/input/` folder
2. Open `lane_detection.ipynb` in Jupyter Notebook or VS Code
3. Update the `image_path` variable to point to your road image:
```python
image_path = 'images/input/road1.jpg'
```
4. Run all cells to process the image
5. Detected lane images will be saved in `images/output/`

## How It Works

### Pipeline Steps:

1. **Load Image**: Read road image and convert from BGR to RGB
2. **Grayscale Conversion**: Convert to grayscale for edge detection
3. **Median Filtering**: Reduce noise while preserving edges
   - Kernel sizes: 3, 5, or 9 (depends on image quality)
4. **Canny Edge Detection**: Detect strong edges using double threshold
   - Lower threshold: 150
   - Upper threshold: 300
5. **Define ROI**: Create trapezoidal mask to focus on lane area
6. **Apply ROI Mask**: Keep only edges within the region of interest
7. **Hough Transform**: Detect straight lines from edge patterns
   - Uses Probabilistic Hough Transform (HoughLinesP)
8. **Draw Lines**: Overlay detected lanes on original image in red
9. **Save Output**: Export final lane detection result

### Key Parameters:

#### Canny Edge Detection:
- **Lower Threshold (150)**: Minimum gradient for weak edges
- **Upper Threshold (300)**: Minimum gradient for strong edges
- **Ratio**: 2:1 (standard for edge connectivity)

#### Hough Transform:
- **rho = 1**: Distance resolution (1 pixel precision)
- **theta = π/180**: Angular resolution (1 degree)
- **threshold = 50**: Minimum votes to detect a line
- **minLineLength = 100**: Minimum line length in pixels
- **maxLineGap = 50-60**: Maximum gap to connect line segments

#### ROI Configuration:
Different road types require different ROI shapes:
- **Highway (road1)**: `[(0, height), (width*0.3, height*0.6), (width*0.7, height*0.6), (width, height)]`
- **City road (road3)**: `[(0, height), (width*0.35, height*0.3), (width*0.65, height*0.3), (width, height)]`

## Technical Details

### Why Canny Edge Detector?

Canny is preferred over Laplacian or Sobel for lane detection because:
- **Thin edges**: 1-pixel wide edges for precise localization
- **Noise resistance**: Built-in Gaussian smoothing
- **Edge connectivity**: Hysteresis connects broken edges
- **Multi-stage**: Superior to single-stage detectors

Comparison:
| Feature | Canny | Laplacian | Sobel |
|---------|-------|-----------|-------|
| Edge Width | 1 pixel | Thick | Moderate |
| Noise Handling | Excellent | Poor | Good |
| Precision | High | Low | Moderate |
| Best For | Object boundaries | Textures | Gradients |

### Why Hough Transform?

Hough Transform excels at detecting straight lines by:
1. **Voting mechanism**: Edge pixels vote for possible lines
2. **Parameter space**: Transforms to (ρ, θ) representation
3. **Robustness**: Works even with gaps and noise
4. **Line connectivity**: `maxLineGap` connects dashed lanes

### Parameter Tuning:

**For different road conditions:**
- **Faded lanes**: Lower `threshold` (30-40) and Canny thresholds
- **Dashed lanes with large gaps**: Increase `maxLineGap` (70-100)
- **Busy roads**: Adjust ROI to exclude distractions
- **Different resolutions**: Scale `minLineLength` and `maxLineGap` proportionally

## Examples

The `images/input/` folder contains sample road images:
- `road1.jpg` - Highway with clear lane markings
- `road2.jpg` - Urban road
- `road3.jpg` - City road with multiple lanes

Corresponding outputs are saved in `images/output/`.

## Project Structure

```
lane-detection/
├── lane_detection.ipynb      # Main Jupyter notebook
├── README.md                  # This file
├── requirements.txt           # Python dependencies
└── images/
    ├── input/                # Place road images here
    └── output/               # Detected lane images saved here
```

## Visualization Features

The notebook includes:
- Step-by-step visualization of each processing stage
- Comparison of different filter kernel sizes
- Hough accumulator array visualization (voting heatmap)
- ROI mask visualization
- Final lane overlay on original image

## Notes

- Works best with clear, well-lit road images
- Adjust ROI vertices based on camera perspective
- High Canny thresholds (150, 300) are tuned for bright lane markings
- Experiment with median kernel sizes (3, 5, 9) for different noise levels
- Hough parameters may need tuning for different road types

## Applications

- Autonomous vehicle lane keeping
- Lane departure warning systems
- Advanced driver assistance systems (ADAS)
- Road infrastructure analysis
- Traffic monitoring