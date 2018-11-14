# This repo contains tools for data parsing for various datasets.


## Data Collection & Parsing
Important Notes:
1. Please open all python scripts and read the instructions carefully.
2. Different datasets will have different Annotation formats. For example, COCO has “.txt” format Annotation file for each corresponding Image, same for OPEN IMAGES V4. Pascal and Custom dataset follows “.xml” format.
3. In order to make everything simple, we convert all the dataset to single “CSV” format for each object.

Desired CSV Annotation format:

Example Dog.csv

| Filename | class | width | Height | xmin | ymin | xmax | ymax |
|----------|-------|-------|--------|------|------|------|------|
|  A.jpg   |  Dog  |  1080 |  920   |  736 | 876  |  999 |  998 |
|  B.jpg   |  Dog  |  1080 |  920   |  272 | 290  |  789 |  678 |


4. **Naming Conventions**:  Choose a unique name for every object. For example, COCO has object called Cell_phone and OID has object called Mobile_phone. In this case both the objects are same but with a different name. So, you need to choose a unique name for the object either Cell_phone or Mobile_phone. In order to rename the object, you can use rename_class_csv.py. You need to execute this script on final csv file and then use the modified(renamed object) csv file to convert into TFRecords. For example, in COCO dataset you want to rename Cell_phone to Mobile_phone, use **rename_class_csv.py** with final Cell_phone.csv and you can find newely generated Mobile_phone.csv in where Cell_phone.csv is residing. Then use Mobile_phone.csv to convert into TFRecords.

**Note:** We collected and created customized dataset from COCO, OPENIMAGES V4 and Honda custom data for CDCA.





## COCO

### Downloading and arranging the dataset
To download COCO dataset, please go to this link https://pjreddie.com/projects/coco-mirror/ and download “2014 Training images [80K/13GB]”, “2014 Val. images [40K/6.2GB], "labels.tgz" and unzip everything.


After you download all the required dataset, create a folder called **COCO** with subfolders **images** and **annotations**. Now move unzipped images to “images” folder and unzipped labels to “annotations" folder.

**Remove “2014” in subfolders names of images and annotations i;e train2104 to train and val2014 to val.**

Please clone all the files from CDCA\datasets\SSD\dataset_tools\COCO and move to “COCO” folder you created.

After moving all the required files and folders **COCO** folder should look like below:
```
    --COCO
	--coco_id.txt
	--COCO_CDCA_classes.txt
	--create_CDCA_train_TFRecords.sh
	--create_CDCA_val_TFRecords.sh
	--init_csvs_coco_txt_csv.py
	--img_W_H.py
	--merge_final_csv.py
	--tfrecords.py
	--annotations
		--train
		--val
	--images
		--train
		--val
```

Before starting the data parsing, please go to "./COCO/annotations/" & "./COCO/images" and have a look at the format of annotations and images.

### 1 Converting annotations from “txt” to “csv”

In order to modify all the annotations to “CSV” format for each object we use **init_csvs_coco_txt_csv.py**. When you Execute init_csvs_coco_txt_csv.py for “train” and “val”, a new folder called **“init_csvs”** with the subfolders **“init_train”** & **“init_val”** is created and all the “CSV” files are stored in the corresponding folders.
```
#From ./COCO/

python init_csvs_coco_txt_csv.py --dataset "./annotations/train/" --type "train"

python init_csvs_coco_txt_csv.py --dataset "./annotations/val/" --type "val"

```
After you finish converting the annotations from "txt" to "csv", please have a look at annotations in **"./COCO/init_csvs/init_train"** & **"./COCO/init_csvs/init_val"** .You will find height and widths of the images are missing.

### 2 Getting Image widths and heights

In order to get width and height of the image, please use **img_W_H.py**. When you execute this file for “train” and “val”, you will find a new folder named **“img_W_H”** and **“train_coco.csv”** and **“val_coco.csv”** files for corresponding train or val. This csv files contains all the image filenames along with their corresponding width and height.

```
#From ./COCO/

python img_W_H.py --path "./images/train/" --type "train"

python img_W_H.py --path "./images/val/" --type "val"

```

### 3 Merging initial_csv and img_W_H files

Now using **merge_final_csv.py** merge initial_csv & img_W_H files for corresponding train and val in order to get all the required fields into single csv file for each object. This final CSV files are stored in **final_csv"** folder with corresponding **train** and **val** folders.

```
#From ./COCO/

python merge_final_csv.py --type "train"

python merge_final_csv.py --type "val"

```

### 4 Renaming object names

For this step please refer to **Naming Conventions** in **Data Collection & Parsing** Section.

For CDCA, we need to rename  mouse to Computer_mouse, remote to Remote_control, cell_phone to Mobile_phone.

```
python rename_class_csv.py --path "./final_csv/train/" --frm "Cell_phone" --to "Mobile_phone"
python rename_class_csv.py --path "./final_csv/train/" --frm "Mouse" --to "Computer_mouse"
python rename_class_csv.py --path "./final_csv/train/" --frm "Remote" --to "Remote_control"

python rename_class_csv.py --path "./final_csv/val/" --frm "Cell_phone" --to "Mobile_phone"
python rename_class_csv.py --path "./final_csv/val/" --frm "Mouse" --to "Computer_mouse"
python rename_class_csv.py --path "./final_csv/val/" --frm "Remote" --to "Remote_control"

```

### 5 Converting final csv files to TFRecords

In order to convert CSV to TFRecords you need to have list of objects in a txt file, you can find objects used for CDCA project in **COCO_CDCA_classes.txt** file or you can create your own “txt” file including all your desired objects. You can find all coco object list in coco_id.txt file.
Create a new folder **TFRecords** along with subfolders **train** and **val** in **COCO** folder. 
Now we are ready to convert all this final csv files to TFRecords. Using **tfrecords.py** file, convert all the annotations to TFRecords format.

For CDCA
```
#From ./COCO/

./create_CDCA_train_TFRecords.sh

./create_CDCA_val_TFRecords.sh
```
After you finish running the shell scripts, you will find all the TFRecords required for the training in **TFRecords** folder. Using this TFRecords you are all set for training.

After you finished all the above steps. COCO folder looks like this.

```
    --COCO
	--coco_id.txt
	--COCO_CDCA_classes.txt
	--create_CDCA_train_TFRecords.sh
	--create_CDCA_val_TFRecords.sh
	--init_csvs_coco_txt_csv.py
	--img_W_H.py
	--no_of_images.py
	--merge_final_csv.py
	--tfrecords.py
	--init_csvs
		--init_train
		--init_val
	--img_W_H
		--train_coco.csv
		--val_coco.csv
	--final_csv
		--train
		--val
	--TFRecords
		--train
		--val
	--Annotations
		--train Annotations
		--val Annotations 
	--Images
		--train Images
		--val Images
```

## OPEN IMAGES DATASET

### 1.Downloading OID_V4 dataset
To download specific classes from OPEN IMAGES Dataset, clone the script from https://github.com/EscVM/OIDv4_ToolKit.

After you clone, replace **OIDv4_Toolkit/modules/downloader.py** with **CDCA\datasets\SSD\dataset_tools\OpenImages\downloader.py**

Download **classes-descriptions-boxable.csv, train-annotations-bbox.csv, test-annotations-bbox.csv, val-annotations-bbox.csv** files from https://storage.googleapis.com/openimages/web/download.html (check section **Subset with Bounding Boxes (600 classes)/ Boxes & Metadata** in the link to download files)

Copy downloaded files **classes-descriptions-boxable.csv, train-annotations-bbox.csv, test-annotations-bbox.csv, val-annotations-bbox.csv** to **OIDv4_Toolkit/OID/csv_folder/**

From **./OIDv4_Toolkit**(where main.py is residing) folder use:
```
python3 main.py downloader --classes "specify your desired classes here" --type_csv "dataset type"
```

For CDCA use below scripts for train, validation & test
```
python3 main.py downloader --classes Toy Beer Chopsticks Towel Glove Sunglasses Ball Backpack Headphones Fast_food Screwdriver Laptop Person Wrench Flashlight Scissors Suitcase Snack Medical_equipment Cat Computer_mouse Coin Calculator Box Stapler Drink Ratchet Hat Eraser Tin_can Mug Can_opener Goggles Coffee_cup Paper_towel Flying_disc Face_powder Fruit Pillow Hammer Drinking_straw Hair_dryer Alarm_clock Knife Bottle Bottle_opener Dumbbell Bowl Musical_instrument Ring_binder Plate Mobile_phone Crutch Pencil_case Briefcase Plastic_bag Sports_equipment Lipstick High_heels Shotgun Picture_frame Tripod Picnic_basket Handbag Toilet_paper Footwear Tablet_computer Dog Book Axe Flower Spoon Fork Camera Vegetable Diaper Envelope Watch Handgun Facial_tissue_holder Ruler Luggage_and_bags Umbrella Glasses Pen Binoculars Perfume Remote_control Helmet --type_csv train

python3 main.py downloader --classes Toy Beer Chopsticks Towel Glove Sunglasses Ball Backpack Headphones Fast_food Screwdriver Laptop Person Wrench Flashlight Scissors Suitcase Snack Medical_equipment Cat Computer_mouse Coin Calculator Box Stapler Drink Ratchet Hat Eraser Tin_can Mug Can_opener Goggles Coffee_cup Paper_towel Flying_disc Face_powder Fruit Pillow Hammer Drinking_straw Hair_dryer Alarm_clock Knife Bottle Bottle_opener Dumbbell Bowl Musical_instrument Ring_binder Plate Mobile_phone Crutch Pencil_case Briefcase Plastic_bag Sports_equipment Lipstick High_heels Shotgun Picture_frame Tripod Picnic_basket Handbag Toilet_paper Footwear Tablet_computer Dog Book Axe Flower Spoon Fork Camera Vegetable Diaper Envelope Watch Handgun Facial_tissue_holder Ruler Luggage_and_bags Umbrella Glasses Pen Binoculars Perfume Remote_control Helmet --type_csv validation

python3 main.py downloader --classes Toy Beer Chopsticks Towel Glove Sunglasses Ball Backpack Headphones Fast_food Screwdriver Laptop Person Wrench Flashlight Scissors Suitcase Snack Medical_equipment Cat Computer_mouse Coin Calculator Box Stapler Drink Ratchet Hat Eraser Tin_can Mug Can_opener Goggles Coffee_cup Paper_towel Flying_disc Face_powder Fruit Pillow Hammer Drinking_straw Hair_dryer Alarm_clock Knife Bottle Bottle_opener Dumbbell Bowl Musical_instrument Ring_binder Plate Mobile_phone Crutch Pencil_case Briefcase Plastic_bag Sports_equipment Lipstick High_heels Shotgun Picture_frame Tripod Picnic_basket Handbag Toilet_paper Footwear Tablet_computer Dog Book Axe Flower Spoon Fork Camera Vegetable Diaper Envelope Watch Handgun Facial_tissue_holder Ruler Luggage_and_bags Umbrella Glasses Pen Binoculars Perfume Remote_control Helmet --type_csv test
```
Note: There are no images provided for some objects in validation and test datasets.

After you download the dataset. Folder format looks like below.
```
--OIDv4_Toolkit
	--images
	--modules
	--OID
	   --csv_folder
	   --Dataset
		--train
		   --Example_object
		         --Label
		            --Example_object.txt
			    --Example_image1.jpg
		--test
		   --Example_object
		         --Label
		            --Example_object.txt
			    --Example_image1.jpg
		--validation
		   --Example_object
		         --Label
		           --Example_object.txt
			   --Example_image1.jpg
```
**Note: Please rename "/OIDv4_ToolKit/OID/Dataset/validation" to "/OIDv4_ToolKit/OID/Dataset/val"**

### 2.Parsing the dataset

Now Create a new folder called **"OID_v4"**(Where your datasets are residing).

Create a subfolder **"images"** inside "OID_v4"

Now copy the downloaded train, validation, test datasets to **"OID_v4/images"** folder. i;e **"/OIDv4_ToolKit/OID/Dataset/train", "/OIDv4_ToolKit/OID/Dataset/test", "/OIDv4_ToolKit/OID/Dataset/val"** to **"OID_v4/images"**(I copied the folders manually)

Copy **"OID.py", "tfrecords.py", "OID_classes_honda.txt", "create_train_TFRecords.sh", "create_val_TFRecords.sh"** files from github "/CDCA/datasets/SSD/dataset_tools/OpenImages/" to "OID_v4/images"

After Copying all the required file and folders. Folder **"OID_v4"** should look like below.



```
--OID_v4
    --images
	--train
	--test
	--val
    --OID.py
    --tfrecords.py 
    --OID_classes_honda.txt
    --create_train_TFRecords.sh
    --create_val_TFRecords.sh
```

Now you are ready to parse the dataset. Run the below script to combine train,validation and test datasets and divide them into 70-30% train and val datasets along with the annotations.

```
#From ./OID_v4/
Use python OID.py --objlis "Give your desired objects here"
```

For CDCA use 
```
#From ./OID_v4/
python OID.py --objlis 'Drink' 'High_heels' 'Alarm_clock' 'Backpack' 'Ball' 'Beer' 'Bottle' 'Bottle_opener' 'Bowl' 'Box' 'Briefcase' 'Calculator' 'Can_opener' 'Cat' 'Chopsticks' 'Coffee_cup' 'Coin' 'Computer_mouse' 'Crutch' 'Drinking_straw' 'Dumbbell' 'Eraser' 'Face_powder' 'Fast_food' 'Flashlight' 'Flying_disc' 'Footwear' 'Fruit' 'Glove' 'Goggles' 'Hair_dryer' 'Hammer' 'Handbag' 'Hat' 'Headphones' 'Knife' 'Laptop' 'Lipstick' 'Medical_equipment' 'Mobile_phone' 'Mug' 'Musical_instrument' 'Paper_towel' 'Pencil_case' 'Picnic_basket' 'Picture_frame' 'Pillow' 'Plastic_bag' 'Plate' 'Ratchet' 'Ring_binder' 'Scissors' 'Screwdriver' 'Shotgun' 'Snack' 'Sports_equipment' 'Stapler' 'Suitcase' 'Sunglasses' 'Tin_can' 'Toilet_paper' 'Towel' 'Toy' 'Tripod' 'Wrench' 'Axe' 'Binoculars' 'Book' 'Camera' 'Diaper' 'Dog' 'Envelope' 'Facial_tissue_holder' 'Flower' 'Fork' 'Glasses' 'Handgun' 'Helmet' 'Luggage_and_bags' 'Pen' 'Perfume' 'Remote_control' 'Ruler' 'Spoon' 'Tablet_computer' 'Umbrella' 'Vegetable' 'Watch'
```

After you execute the OID.py script, you will find new folders created inside "./OID_v4".

"./OID_v4" folder format looks like below.

```
--OID_v4
    --images
	--train
	--test
	--validation(please rename this folder name from "validation" to "val")
    --annotations (You will find train, test, val merged annotations)
    --OID_CDCA
	--annotations (You will find split 70-30% train & val annotations)
	     --train
	     --val
	--images (You will find split 70-30% train & val images)
	     --train
	     --val
    --TFRecords
	--train
	--val
    --OID.py
    --tfrecords.py 
    --OID_classes_honda.txt 
    --create_train_TFRecords.sh 
    --create_val_TFRecords.sh
```

### 3.Creating TFRecords

After you finish parsing the dataset, you need to create **TFRecords**.

```
#From "./OID_v4"
Run the shell scripts
./create_train_TFRecords.sh
./create_val_TFRecords.sh
```

After you finish running the shell scripts you will find train & val TFRecords in "./OID_v4/TFRecords" folder.

Now you are all set for training OPEN IMAGES DATASET.

## Custom dataset

In order to create a custom dataset, select the desired object with different colors and different sizes. Take the pictures of the objects in various angles with a decent camera.

**Downloading Images from google:**
In order to download images from google for your desired object, please follow this link https://github.com/hardikvasa/google-images-download and follow the instructions.

In order to download more than 100 images for a specific object, please follow Installing the chromedriver (with Selenium) in the above link.

After downloading the images, please check the images manually whether the corrupted images are present or not. If you find corrupted images, please delete it.

###Arranging dataset

After you finish collecting the dataset, create a folder called “Custom_dataset” and copy all the files from github repo \CDCA\datasets\SSD\dataset_tools\Custom_dataset.

Create a folder called **JPEGImages** inside **Custom_dataset** folder and move all the collected images to **JPEGImages**

After arranging everything **Custom_dataset** should like below.

```
--Custom_dataset
	--JPEGImages
		--A.jpg
		--B.jpg
	--change_file_extension.py
	--FrameFix.py
	--FrameTag.py
	--GenValid.py
	--movesimages.py
	--tfrecords.py
	--xml_to_csv.py
	--custom_classes.txt
```

### 1.Image format

We follow “jpg” format for images. Use **change_file_extension.py** to change the custom images to “jpg” format.

```
#From ./Custom_dataset

Example: python change_file_extension.py --frm JPG --to jpg --folder './JPEGImages/'
```

### 2.Image tagging
After you collect the dataset, you are ready to tag and create the annotations. To tag the pictures please use annotation tool **FrameTag.py**.

```
#From ./Custom_dataset

python FrameTag.py

```

### 3.Moving Images
While tagging if you close the tagging tool accidentally or you close the tagging tool before you finish tagging all the images, there will be mismatch in no.of annotations to no.of.images. In order to overcome mismatch use **movesimages.py** and restart the tagging.

```
#From ./Custom_dataset

python **moveimages.py**
```

**Note:** After you finsh tagging all the images please move all the images from **Parsed_Images** back to **JPEGImages**

### 4.Custom_classes.txt
**Custom_classes.txt**, please include all the objects you tagged in this file.


### 5.Validating Annotations 
FrameTag.py annotation tool generates the annotations in “xml” format. In order to validate the Tagged annotations use **FrameFix.py**, after you execute this file you will see two new folders **“Corrupted_Annotations”** & **“Corrupted_Images”** along with Corrupted Annotations and Images in it. The corrupted annotations should be excluded from training otherwise you might get issues during training.

```
#From ./Custom_dataset

python FrameFix.py --folder "./Annotations/"

```

### 6.Dividing the dataset into Train, validation.
After validating the Annotations, you need to divide the dataset into train and validation set. Use **GenValid.py** script to do this task. After executing GenValid.py file, you will find train and val folders along with images & annotations in it.

```
#From ./Custom_dataset

python GenValid.py --output "./Validation/" --dataset "./JPEGImages/" --annotations "./Annotations/" --percentage 0.3

```

### 7.Converting XML to CSV
After generating train and validation sets, you need to convert them into “csv” format. Use **xml_to_csv.py** to do this task. After executing the script you will find a new folder **“final_csv”** along with **train** and **validation** csv files in it. This CSV files are used to convert into TFRecords.

``` 
#From ./Custom_dataset

python -obj "./train/"

python -obj "./val/"

### 8.CSV to TFRecords
Now we need to convert these csv files into TFRecords, use **tfrecords.py** file finish this task. 

Create a new folder **TFRecords** along with subfolders **train** and **val** in **Custom_dataset** folder manually. 

```
#From ./Custom_dataset

python tfrecords.py --csv_input=train.csv  --output_path=train.record --im_path ./train/

python tfrecords.py --csv_input=val.csv  --output_path=val.record --im_path ./val/
```
After executing **tfrecords.py**, you will find **tfrecords** in **TFRecords** folder.

After you finish the above procedures, Custom_dataset folder looks like below.

```
Custom_dataset
	--JPEGImages
		--A.jpg
		--B.jpg
	--Annotations
	             --A.xml
	             --B.xml
	--TFRecords
		--train
		--val
	--final_csv
		--train.csv
		--test.csv
	--train
	--val
	--change_file_extension.py
	--FrameFix.py
	--FrameTag.py
	--GenValid.py
	--movesimages.py
	--tfrecords.py
	--xml_to_csv.py
	--custom_classes.txt
```

Now everything is ready to start the training.

**Note:** For maintaing the dataset folders clean, after finishing all the above procedures, craete a folder **Tagged date** for ex: "Sep11", move manually **JPEGImages, Annotations, TFRecords, final_csv, train, val** folders to the **Tagged date** folder.

