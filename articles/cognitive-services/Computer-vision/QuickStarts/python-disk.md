---
title: "Quickstart: Analyze a local image - REST, Python"
titleSuffix: "Azure Cognitive Services"
description: In this quickstart, you analyze a local image using the Computer Vision APi with Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme

ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: pafarley
ms.custom: seodec18
---
# Quickstart: Analyze a local image using the REST API and Python in Computer Vision

In this quickstart, you analyze a locally stored image to extract visual features by using Computer Vision's REST API. With the [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) method, you can extract visual features based on image content.

You can run this quickstart in a step-by step fashion using a Jupyter notebook on [MyBinder](https://mybinder.org). To launch Binder, select the following button:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/try/cognitive-services/) before you begin.

## Prerequisites

- You must have [Python](https://www.python.org/downloads/) installed if you want to run the sample locally.
- You must have a subscription key for Computer Vision. To get a subscription key, see [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md).

## Create and run the sample

To create and run the sample, do the following steps:

1. Copy the following code into a text editor.
1. Make the following changes in code where needed:
    1. Replace the value of `subscription_key` with your subscription key.
    1. Replace the value of `vision_base_url` with the endpoint URL for the Computer Vision resource in the Azure region where you obtained your subscription keys, if necessary.
    1. Optionally, replace the value of `image_path` with the path and file name of a different image that you want to analyze.
1. Save the code as a file with an `.py` extension. For example, `analyze-local-image.py`.
1. Open a command prompt window.
1. At the prompt, use the `python` command to run the sample. For example, `python analyze-local-image.py`.

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the "westus" region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

analyze_url = vision_base_url + "analyze"

# Set image_path to the local path of an image that you want to analyze.
image_path = "C:/Documents/ImageToAnalyze.jpg"

# Read the image into a byte array
image_data = open(image_path, "rb").read()
headers    = {'Ocp-Apim-Subscription-Key': subscription_key,
              'Content-Type': 'application/octet-stream'}
params     = {'visualFeatures': 'Categories,Description,Color'}
response = requests.post(
    analyze_url, headers=headers, params=params, data=image_data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The most
# relevant caption for the image is obtained from the 'description' property.
analysis = response.json()
print(analysis)
image_caption = analysis["description"]["captions"][0]["text"].capitalize()

# Display the image and overlay it with the caption.
image = Image.open(BytesIO(image_data))
plt.imshow(image)
plt.axis("off")
_ = plt.title(image_caption, size="x-large", y=-0.1)
```

## Examine the response

A successful response is returned in JSON. The sample webpage parses and displays a successful response in the command prompt window, similar to the following example:

```json
{
  "categories": [
    {
      "name": "outdoor_",
      "score": 0.00390625,
      "detail": {
        "landmarks": []
      }
    },
    {
      "name": "outdoor_street",
      "score": 0.33984375,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "building",
      "outdoor",
      "street",
      "city",
      "people",
      "busy",
      "table",
      "walking",
      "traffic",
      "filled",
      "large",
      "many",
      "group",
      "night",
      "light",
      "crowded",
      "bunch",
      "standing",
      "man",
      "sign",
      "crowd",
      "umbrella",
      "riding",
      "tall",
      "woman",
      "bus"
    ],
    "captions": [
      {
        "text": "a group of people on a city street at night",
        "confidence": 0.9122243847383961
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Brown",
    "dominantColorBackground": "Brown",
    "dominantColors": [
      "Brown"
    ],
    "accentColor": "B54316",
    "isBwImg": false
  },
  "requestId": "c11894eb-de3e-451b-9257-7c8b168073d1",
  "metadata": {
    "height": 600,
    "width": 450,
    "format": "Jpeg"
  }
}
```

## Clean up resources

When no longer needed, delete the file.

## Next steps

Explore a Python application that uses Computer Vision to perform optical character recognition (OCR); create smart-cropped thumbnails; plus detect, categorize, tag, and describe visual features, including faces, in an image. To rapidly experiment with the Computer Vision API, try the [Open API testing console](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Computer Vision API Python Tutorial](../Tutorials/PythonTutorial.md)
