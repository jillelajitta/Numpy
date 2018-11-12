This repo contains tools for data parsing for various datasets.

OPEN IMAGES DATASET

To download specific classes from OPEN IMAGES Dataset, clone the script from https://github.com/EscVM/OIDv4_ToolKit.
After you clone, replace OIDv4_Toolkit/modules/downloader.py with CDCA\datasets\SSD\dataset_tools\OpenImages\downloader.py
Copy classes-descriptions-boxable.csv, train-annotations-bbox.csv, test-annotations-bbox.csv, val-annotations-bbox.csv files from CDCA\datasets\SSD\dataset_tools\OpenImages\ to OIDv4_Toolkit/OID/csv_folder/

From ./OIDv4_Toolkit(where main.py is residing) folder
Use python3 main.py downloader.py --classes "specify your desired classes here" --type_csv "dataset type"

For CDCA use below scripts for train, validation & test

python3 main.py downloader --classes Toy Beer Chopsticks Towel Glove Sunglasses Ball Backpack Headphones Fast_food Screwdriver Laptop Person Wrench Flashlight Scissors Suitcase Snack Medical_equipment Cat Computer_mouse Coin Calculator Box Stapler Drink Ratchet Hat Eraser Tin_can Mug Can_opener Goggles Coffee_cup Paper_towel Flying_disc Face_powder Fruit Pillow Hammer Drinking_straw Hair_dryer Alarm_clock Knife Bottle Bottle_opener Dumbbell Bowl Musical_instrument Ring_binder Plate Mobile_phone Crutch Pencil_case Briefcase Plastic_bag Sports_equipment Lipstick High_heels Shotgun Picture_frame Tripod Picnic_basket Handbag Toilet_paper Footwear Tablet_computer Dog Book Axe Flower Spoon Fork Camera Vegetable Diaper Envelope Watch Handgun Facial_tissue_holder Ruler Luggage_and_bags Umbrella Glasses Pen Binoculars Perfume Remote_control Helmet --type_csv train

python3 main.py downloader --classes Toy Beer Chopsticks Towel Glove Sunglasses Ball Backpack Headphones Fast_food Screwdriver Laptop Person Wrench Flashlight Scissors Suitcase Snack Medical_equipment Cat Computer_mouse Coin Calculator Box Stapler Drink Ratchet Hat Eraser Tin_can Mug Can_opener Goggles Coffee_cup Paper_towel Flying_disc Face_powder Fruit Pillow Hammer Drinking_straw Hair_dryer Alarm_clock Knife Bottle Bottle_opener Dumbbell Bowl Musical_instrument Ring_binder Plate Mobile_phone Crutch Pencil_case Briefcase Plastic_bag Sports_equipment Lipstick High_heels Shotgun Picture_frame Tripod Picnic_basket Handbag Toilet_paper Footwear Tablet_computer Dog Book Axe Flower Spoon Fork Camera Vegetable Diaper Envelope Watch Handgun Facial_tissue_holder Ruler Luggage_and_bags Umbrella Glasses Pen Binoculars Perfume Remote_control Helmet --type_csv validation

python3 main.py downloader --classes Toy Beer Chopsticks Towel Glove Sunglasses Ball Backpack Headphones Fast_food Screwdriver Laptop Person Wrench Flashlight Scissors Suitcase Snack Medical_equipment Cat Computer_mouse Coin Calculator Box Stapler Drink Ratchet Hat Eraser Tin_can Mug Can_opener Goggles Coffee_cup Paper_towel Flying_disc Face_powder Fruit Pillow Hammer Drinking_straw Hair_dryer Alarm_clock Knife Bottle Bottle_opener Dumbbell Bowl Musical_instrument Ring_binder Plate Mobile_phone Crutch Pencil_case Briefcase Plastic_bag Sports_equipment Lipstick High_heels Shotgun Picture_frame Tripod Picnic_basket Handbag Toilet_paper Footwear Tablet_computer Dog Book Axe Flower Spoon Fork Camera Vegetable Diaper Envelope Watch Handgun Facial_tissue_holder Ruler Luggage_and_bags Umbrella Glasses Pen Binoculars Perfume Remote_control Helmet --type_csv test

Note: There are no images provided for some objects in validation and test datasets.

After you download the dataset. Folder format looks like below.

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

Now Create a new folder called "OID_v4"(Where your datasets are residing).
Create a subfolder "images" inside "OID_v4"

Now copy the downloaded train, validation, test datasets to "OID_v4/images" folder. i;e "/OIDv4_ToolKit/OID/Dataset/train", "/OIDv4_ToolKit/OID/Dataset/test", "/OIDv4_ToolKit/OID/Dataset/validation" to "OID_v4/images"

Copy "OID.py", "tfrecords.py", "OID_classes_honda.txt", "create_train_TFRecords.sh", "create_val_TFRecords.sh" files from github "/CDCA/datasets/SSD/dataset_tools/OpenImages/" to "OID_v4/images"

After Copying all the required file and folders. Folder "OID_v4" should look like below.

--OID_v4
    --images
	--train
	--test
	--validation(please rename this folder name from "validation" to "val")
    --OID.py
    --tfrecords.py 
    --OID_classes_honda.txt 
    --create_train_TFRecords.sh 
    --create_val_TFRecords.sh

Now you are ready to parse the dataset.
From ./OID_v4/
	
