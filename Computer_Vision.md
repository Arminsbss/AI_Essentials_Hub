# Computer Vision Essentials

Computer vision turns visual inputs into classifications, detections, masks, tracks, or structured information. Separate measurement and recognition from image generation; they require different evaluation criteria.

## A practical toolkit

OpenCV handles image loading, transformations, geometry, and video operations. Torchvision supplies model architectures, pretrained weights, and associated transforms. Ultralytics is an option for detection, segmentation, and tracking workflows. These roles overlap, but none is a universal replacement for the others.[^16][^15][^17]

The older ImageAI RetinaNet `.h5` example has been removed from the default path. This is a curriculum decision to use explicit, documented model and preprocessing APIs; it is not a verified claim that every ImageAI project is abandoned.

| Problem | Start with | Measure |
|---|---|---|
| Image classification | Pretrained classifier and transfer learning | Per-class precision/recall and errors |
| Object detection | A detector matched to the objects and hardware | Detection metrics plus end-to-end latency |
| Segmentation | A suitable mask model | IoU/Dice and boundary errors |
| Text in documents | OCR or a document-understanding model | Field correctness and source localization |
| Visual question answering | A vision-language model | Grounded accuracy, counting and spatial failures |

## Robust image handling

Requires OpenCV and a local `image.jpg` file. It saves a result and does not require a desktop display window.

```python
from pathlib import Path
import cv2

source = Path("image.jpg")
image = cv2.imread(str(source))
if image is None:
    raise ValueError(f"Could not decode image: {source}")
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
if not cv2.imwrite("image_gray.png", gray):
    raise OSError("Could not write image_gray.png")
print("Input shape:", image.shape, "Output shape:", gray.shape)
```

Check loading failures and channel conventions. OpenCV commonly represents color images as BGR, while many model pipelines expect RGB. Shape and color errors can silently degrade predictions.[^16]

## Pretrained models

Use the weight-specific transforms supplied with a Torchvision checkpoint. Resize, crop, normalization, and category mapping should travel with the weights. For a reproducible deployment, pin the exact weight enum or checkpoint revision rather than depending on a changing `DEFAULT` alias.[^15]

Freeze most layers for a first transfer-learning experiment, then compare partial fine-tuning. Do not apply augmentation to evaluation data unless it is a deliberate, documented test-time method.

## Data and deployment

Split by original image, patient, site, camera, or recording as appropriate. Adjacent video frames in train and test sets can produce misleading scores. Examine lighting, blur, occlusion, background changes, and rare objects. Measure preprocessing and data-transfer time as well as model inference.

Review the actual library and checkpoint licenses before redistribution. Ultralytics documents AGPL-3.0 and enterprise licensing options; the repository's MIT license does not override upstream terms.[^17]

**Exercise:** Evaluate a pretrained classifier on 30 examples from the intended environment, including hard failures. Record the weight revision and transformations, then determine whether a more complex model addresses the observed errors.

[Back to AI Essentials Hub](README.md)

## Sources

[^15]: Torchvision maintainers. [Models and pre-trained weights](https://docs.pytorch.org/vision/stable/models.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^16]: OpenCV contributors. [Getting Started with Images](https://docs.opencv.org/4.13.0/db/deb/tutorial_display_image.html). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.

[^17]: Ultralytics. [YOLO Object Detection and Segmentation](https://docs.ultralytics.com/). Living documentation; no fixed publication date. Reviewed 2026-09-12–2026-09-13.
