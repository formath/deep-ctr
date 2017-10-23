# tensorflow-predictor-cpp
TensorFlow prediction using its C++ API.

Having this repo, you will not need `TensorFlow-Serving`. This project has been tested on OSX.


Contains two examples:
* simple model `c = a * b`
* an industrial deep model for large scale click through rate prediction

Covered knowledge points:
* save model and checkpoint
* freeze model with checkpoint
* replace part of nodes in the model for prediction
* transform libfm data into tfrecord
* load model in C++
* construct `SparseTensor` in C++
* prediction in C++

# Build

## Build TensorFlow
Follow the instruction [build tensorflow from source](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/makefile)
```bash
git clone --recursive https://github.com/tensorflow/tensorflow.git
cd tensorflow
sh tensorflow/contrib/makefile/build_all_linux.sh (works for linux and osx)
cd ..
```

## Build this repo
Keep this repo in the same directory with tensorflow.
```bash
git clone https://github.com/formath/tensorflow-predictor-cpp.git
cd tensorflow-predictor-cpp
mkdir build && cd build
cmake ..
make
```

# Simple Model
This demo used `c = a * b` to show how to save the model and load it using C++ for prediction.
* Save model
* Load model
* Prediction

More detail in Chinese: [tensorflow_c++_api_prediction](http://mathmach.com/2017/10/09/tensorflow_c++_api_prediction_first/)
```bash
cd demo/simple_model
# train
sh train.sh
# predict
sh predict.sh
```
If works right, you will see this
```
Session created successfully
Load graph protobuf successfully
Add graph to session successfully
Run session successfully
Tensor<type: float shape: [] values: 6>
output value: 6
```

# Deep CTR Model
This demo show a real-world deep model usage in click through rate prediction.
* Transform LibFM data into TFRecord
* Save model and checkpoint
* Replace parts of model and freeze graph with checkpoint
* Load model and checkpoint
* Prediction

More detail in Chinese: [tensorflow_c++_api_prediction](http://mathmach.com/2017/10/11/tensorflow_c++_api_prediction_second/)

## Transform LibFM data into TFRecord
* LibFM format: `label fieldId:featureId:value ...`
```bash
cd demo/deep_model
sh trans_data_to_tfrecord.sh
```

## Train model
```bash
sh train.sh
```

## Freeze model
```bash
sh freeze_graph.sh
```

## Predict using C++
```bash
sh predict.sh
```
If works right, you will see this
```
load feature num: 34
fieldid: 6 missid: 9
fieldid: 152 missid: 10
fieldid: 9 missid: 3
fieldid: 179 missid: 10
fieldid: 116 missid: 2
fieldid: 179 feanum: 11
fieldid: 116 feanum: 3
fieldid: 6 feanum: 10
fieldid: 152 feanum: 11
fieldid: 9 feanum: 4
Session created successfully
Load graph protobuf successfully
Add graph to session successfully
Run session successfully
Tensor<type: float shape: [1,1] values: [0.055317685]>
output value: 0.0553177
```


