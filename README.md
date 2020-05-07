# understand.ai Anonymizer

To improve privacy and make it easier for companies to comply with GDPR, understand.ai
open-sourced their anonymization software and weights for a model trained on their in-house datasets 
for faces and license plates.
This code repository is made tensorflow 2.0 compatible by fixing various issues in original code.

To make it easy for everyone to use these weights in their own projects the model is trained with 
[Tensorflow Object Detection API](https://github.com/tensorflow/models/tree/master/research/object_detection).

Our anonymizer is used for projects with some of Germany's largest car manufacturers and suppliers,
but we are sure there are many more applications.  
We are looking forward to a widespread use and would love to hear your feedback.  

## Examples

![Face_Example Raw](images/Riaz_2.jpg?raw=true "Title")
![Face Anonymized](output/Riaz_2_anonymized.jpg?raw=true "Title")

![License Plate Example Raw](images/coco02.jpg?raw=true "Title")
![License Plate Example Anonymized](output/coco02_anonymized.jpg?raw=true "Title")


## Installation

To install the anonymizer just clone this repository, create a new python3.6 environment and install the dependencies.  
The sequence of commands to do all this is

```bash
python -m venv ~/.virtualenvs/anonymizer
source ~/.virtualenvs/anonymizer/bin/activate

git clone https://github.com/understand-ai/anonymizer
cd anonymizer

pip install --upgrade pip
pip install -r requirements.txt
```

To make sure everything is working as intended run the test suite with the following command

```bash
pytest
```

Running the test cases can take several minutes and is dependent on your GPU (or CPU) and internet speed.  
Some test cases download model weights and some perform inference to make sure everything works as intended.


## Usage

In case you want to run the model on CPU, make sure that you install `tensorflow` instead of `tensorflow-gpu` listed
in the `requirements.txt`.

Since the weights will be downloaded automatically all that is needed to anonymize images is to run

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights
```

from the top folder of this repository. This will save both anonymized images and detection results as json-files to
the output folder.

### Advanced Usage

In case you do not want to save the detections to json, add the parameter `no-write-detections`.
Example:

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights --no-write-detections
```

Detection threshold for faces and license plates can be passed as additional parameters.
Both are floats in [0.001, 1.0]. Example:

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights --face-threshold=0.1 --plate-threshold=0.9
```

By default only `*.jpg` and `*.png` files are anonymized. To for instance only anonymize jpgs and tiffs, 
the parameter `image-extensions` can be used. Example:

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights --image-extensions=jpg,tiff
```

The parameters for the blurring can be changed as well. For this the parameter `obfuscation-kernel` is used.
It consists of three values: The size of the gaussian kernel used for blurring, it's standard deviation and the size
of another kernel that is used to make the transition between blurred and non-blurred regions smoother.
Example usage:

```bash
PYTHONPATH=$PYTHONPATH:. python anonymizer/bin/anonymize.py --input /path/to/input_folder --image-output /path/to/output_folder --weights /path/to/store/weights --obfuscation-kernel="65,3,19"
```

## Attributions

An image for one of the test cases was taken from the COCO dataset.  
The pictures in this README are under an [Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/legalcode) license.
You can find the pictures [here](http://farm4.staticflickr.com/3081/2289618559_2daf30a365_z.jpg) and [here](http://farm8.staticflickr.com/7062/6802736606_ed325d0452_z.jpg).
