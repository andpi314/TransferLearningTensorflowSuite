# TransferLearningTensorflowSuite
A suite of pre-configurated python scripts and folder to easily execute a transfer learning process from a model to create your own model (frozen_graph.pb)

You need to create six folder named as follow (be sure to use the exactly same name stated here):

In TransferLearningTensroflowSuite:

/Csv

/Dataset

/New_model

/Pre_train_model

/Training

To execute transfer learning procedure you have to:
+ have your own dataset of train and test images, with their .xml PASCAL_VOC Format labels, in the Dataset folder
+ run xml_to_csv.py and get train_fornewmodel.csv and test_fornewmodel.csv in the Csv folder
+ modify generate_tfrecord.py (row 20, set row_label to your own label(s) ), run it and get test.record and train.record in the Training Folder
+ modify config.pbtxt with your own label(s)
+ download pre-trained model (work with ssd_mobilenet v1 coco) and put it into Pre_train_model folder
+ set train.config in order to work with your model (few changes needed if you use mobilenet, else create your own config file according to model needs)
+ run train.py using:
    python train.py --logtostderr --train_dir=training/ --pipeline_config_path=training.config
+ check training procedure with:
    tensorboard --logdir=training/ --host localhost --port 8080
+ create frozen graph with (change ???? to checkpoint number you want to use):
    python export_inference_graph.py --input_type image_tensor --pipeline_config_path training.config --trained_checkpoint_prefix training/model.ckpt-???? --output_directory New_model
+ use frozen_inference_graph.pb in your projects
