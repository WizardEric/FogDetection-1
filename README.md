# Fog Detection Using Highway Cameras

This thesis project studied whether or not convolutional neural networks (CNNs) can be used for the purpose of classifying highway images on fog conditions. Additionally, it was researched if adding four meteorological variables (air temperature, relative humidity, dew point and wind speed) to the model resulted in a better performance. A Resnet18 model pretrained on ImageNet was trained for classifying images, resulting in a 91.05% accuracy on the test dataset. A separate 1-hidden layer network was used to input the four meteorological inputs. This network was merged with the Resnet18 network in the fully connected layer, as to have both images and meteorological variables influence the classifications. This network resulted in 92.66% accuracy on the test dataset.

## Project Structure
- A number of packages used for various purposes
	- train: defines code for training and testing of network
		- train_utils.py: module containing code used for training of models
		- test_utils.py: module containing code used for testing of models
    - data: contains data directories and modules for obtaining data
    	- raw_to_semiprocessed.py: module used for acquiring raw data and putting in pandas dataframe
    	- to_processed.py: module used for transforming pandas dataframe to processed data
    - models: define models and their utils
    	- train_utils.py: models and utils
    - helpers: package containing numer of helper modules
    	- data_plotters.py: used for plotting data
    	- model_plotters.py: model training related plots
    	- data_loader.py: contains dataloader to use in training
    	- metrics.py: some metrics
- Two directories for saving models and plots/other images
	- experiments: directory to save trained models to
	- img: directory to save plots and other images to
- notebooks: contains number of notebooks used for conducting experiments
- directory to run scripts from
	- main.py: used for running models
    
 
## Data
To start training models, data has to be processed first. In the command line, navigate to the `data/` folder and run

```
python3 raw_to_semiprocessed.py
```

This acquires the raw data from the KNMI fog database and creates a pandas dataframe for the train/validation dataset images and a pandas dataframe for the test dataset images. After this script has finished run

```
python3 to_processed.py
```

This script takes the pandas dataframes, links the images to them and transforms them to several dictionaries that all contain four numpy arrays: One numpy array containing the images, a numpy array containing labels for the images, a numpy array containing filepaths of images and a numpy array containing the meteorological variables associated with the images.

## Model training
To train a model, navigate to the top-level directory `FogDetection/` and run

```
python3 main.py
```

In the `main.py` file, argparser is used for passing arguments to functions. If you want to specify a different model for training or change other arguments (e.g. Use meteorological variables for training, run using CUDA, number of epochs to run training), this can be done in the main file. After the model training is completed, the models are saved to '.pth.tar' files and are directly tested on the test dataset.
