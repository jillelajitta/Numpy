# This Repo contains info about training SSD+InceptionV2

### Report on Training times

1. **model_main.py:** It evaluates the model for every checkpoint it generated. Basically it generates checkpoint for every 1200k steps with batch size of 32,

Below data is based on 350k steps on COCO metrics.
	Total Evals: 292
```
	Train 98% + Val 2%			4 days approx.
	Train 70% + Val 30%			48 days approx. (~4 hours for each evaluation)
	Train 90% + Val 10%		  	16 days approx.
```

2. **train.py** & **eval.py** are two separate legacy files

 Below data is based on 350k steps on COCO metrics.
```	
	Train 70 %			~3 days 
	Val 30%				Evaluating for every 20k steps i;e total 18 evals for 350k steps-> 3-4 days

	Total Training			~6-7 days.
```

   Below data is based on 350k steps on Pascal & Open Images V2 detection metrics.
	
```
	Train 70 %			~3 days 
	Val 30%				Evaluating for every 20k steps i;e total 18 evals for 350k steps-> 2-3 days

	Total Training			~5-6 days.
```
**Note:** Based on testing, our computer is unable to handle COCO evaluation on VAL30%. CPU utilization is going very high. Pascal & Open Images V2 detection metric evaluation is going good. We choose Open Images V2 detection metric because most of the CDCA dataset is from OPEN IMAGES.


### Arranging the things for training

List of items needed for training the algorithm:
1. TFRecords for train and validation
2. label_map file
3. Configuration file 

Recommended folder format for training

```
    --SSD
	--train.py
	--eval.py
	--model_main.py
	--export_inference_graph.py
	--labels.txt
	+data
	  -label_map file
	  -train TFRecord file
	  -val TFRecord file
	+models
	  + model
	    -Configuration file 
	    +train
	    +eval
```

Please copy all the generated train and val tfrecords manually to **data** folder. 

### Creating labelmap

To generate label map use **label_map_generator.py** file. You can find **label_map_generator.py** in githuib repo **./CDCA/training/**. In order to execute this file you need to have **labels.txt**, **labels.txt** should include all the list of objects you are going to train. Be careful while listing the objects in **labels.txt**. Please copy label_map_generator.py, labels.txt files to SSD folder.

For CDCA
```
#From ./SSD/

python label_map_generator.py --name "honda_custom_label.pbtxt" --path "./labels.txt"

```

### Script for training the model with train.py

```
#From ./SSD/

python train.py \
        --logtostderr \
        --train_dir=path/to/train_dir \
        --pipeline_config_path=pipeline_config.pbtxt

```

### Script for evaluating model with eval.py

```
#From ./SSD/

python eval.py \
        --logtostderr \
        --checkpoint_dir=path/to/checkpoint_dir \
        --eval_dir=path/to/eval_dir \
        --pipeline_config_path=pipeline_config.pbtxt
```

###Script for training with model_main.py

```
#From ./SSD/

python object_detection/model_main.py \
    --pipeline_config_path=${PIPELINE_CONFIG_PATH} \
    --model_dir=${MODEL_DIR} \
    --num_train_steps=${NUM_TRAIN_STEPS} \
    --sample_1_of_n_eval_examples=$SAMPLE_1_OF_N_EVAL_EXAMPLES \
    --alsologtostderr

```

### Freezing the trained weights
After you are done with the training, you need to freeze the weights. Use the below script to freeze the weights.

```
#From ./SSD/

python export_inference_graph.py \
    --input_type image_tensor \
    --pipeline_config_path path/to/ssd_inception_v2.config \
    --trained_checkpoint_prefix path/to/model.ckpt \
    --output_directory path/to/exported_model_directory
```
