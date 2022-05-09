# Vision Transformer

This directory contains the code to train and test the Vision Transformer model,
as described in [this paper](https://arxiv.org/pdf/2010.11929v1.pdf).
In addition to that, it is also possible to train the same model through
distillation using the method described in the Data Efficient Vision Transformer
[paper](https://arxiv.org/pdf/2012.12877v1.pdf).
The model implementation is based on the implementation in
[this](https://github.com/lucidrains/vit-pytorch) github repository.

## Installation
1. Set up Python dependencies.
   ```bash
   $ conda env create -f environment.yml
   $ conda activate vit
   ```
2. Install QPyTorch from source.
    ```bash
    $ pip install git+https://github.com/Tiiiger/QPyTorch.git
    ```
3. Download and setup the dataset in the base directory of the repository.
   Your directory hierarchy should look like this:
   ```bash
   $ pwd
   <...>/myrtle-vision
   $ tree Resisc45
   Resisc45
   |-- images
   |   |-- airplane
   |   |   |-- airplane_001.jpg
   |   |   ...
   |   |   `-- airplane_700.jpg
   ...
   |   `-- wetland
   |       |-- wetland_001.jpg
   |       ...
   |       `-- wetland_700.jpg
   |-- label_map.json
   |-- test_imagepaths.txt
   |-- train_imagepaths.txt
   `-- val_imagepaths.txt
   ```

   Each of the `_imagepaths.txt` files should contain a list of relative
   filepaths of the images in that set, e.g.

   ```
   $ head Resisc45/val_imagepaths.txt
   images/airplane/airplane_490.jpg
   images/airplane/airplane_491.jpg
   images/airplane/airplane_492.jpg
   images/airplane/airplane_493.jpg
   images/airplane/airplane_494.jpg
   images/airplane/airplane_495.jpg
   images/airplane/airplane_496.jpg
   images/airplane/airplane_497.jpg
   images/airplane/airplane_498.jpg
   images/airplane/airplane_499.jpg
   ```

   `label_map.json` should be a mapping from label names (the image directory
   names) to label ids:
   ```json
   {
     "airplane": 0,
     ...
     "wetland": 44
   }
   ```

## Training
0. Follow the installation instructions above.
1. Train the Vision Transformer model using the config file for the specific
   configuration you want to use.
   ```bash
   $ pwd
   <...>/myrtle-vision
   $ python train.py -c train_configs/<config_file>
   ```

## Test
0. Follow the installation instructions above.
1. Run inference and calculate accuracy on the test set using a previously
   trained model.
   ```bash
   $ pwd
   <...>/myrtle-vision
   $ python test.py -c train_configs/<config_file>
   ```

   Note: make sure to set the `checkpoint_path` argument in the config file to
   the path to the trained checkpoint that will be used to evaluate the model
   accuracy.

## Test with Quantization
The following instructions can be used to test both a model trained with
Quantization-Aware Training and a model trained at full precision by applying
Post-Training Quantization.

0. Follow the installation instructions above.
1. Run inference and calculate accuracy on the test set using a quantized model
   previously trained. If the model was trained with Quantization-Aware
   Training, then you need to add the argument `--quantized_ckpt` to the
   command below.
   ```bash
   $ pwd
   <...>/myrtle-vision
   $ python test_quantize.py -c train_configs/<config_file>
   ```

   Note: make sure to set the `checkpoint_path` argument in the config file to
   the path to the trained checkpoint that will be used to evaluate the model
   accuracy. Moreover, make sure to also set the quantization format with the
   `q_format` parameter in the config file.

## Convert to ONNX
The following command can be used to export a Vision Transformer to ONNX.

0. Follow the installation instructions above.
1. Export a pre-trained model to ONNX.
   ```bash
   $ pwd
   <...>/myrtle-vision
   $ python convert_to_onnx.py \
       -c train_configs/<config_file> \
       -o <output file name>
   ```

   Note: make sure to set the `checkpoint_path` argument in the config file to
   the path to the trained checkpoint that will be exported to ONNX.
