# This Repo contains info about training SSD+InceptionV2

### Report on Training times

1. **model_main.py:** It evaluates the model for every checkpoint it generated. Basically it generates checkpoint for every 1200k steps with batch size of 32,

Below data is based on 350k steps on COCO metrics
Total Evals: 292

Train 98% + Val 2%			4 days approx.
Train 70%+Val 30%			48 days approx. (~4 hours for each evaluation)
Train 90%+ Val 10%		  	16 days approx.

2. **train.py** & **eval.py** are two separate legacy files

**Below data is based on 350k steps on COCO metrics.**

Train 70 %			~3 days 
Val 30%				Evaluating for every 20k steps i;e total 18 evals for 350k steps-> 3-4 days

Total Training			~6-7 days.

**Below data is based on 350k steps on Pascal & Open Images V2 detection metrics.**

Train 70 %			~3 days 
Val 30%				Evaluating for every 20k steps i;e total 18 evals for 350k steps-> 2-3 days

Total Training			~5-6 days.


**Note:** Based on testing, our computer is unable to handle COCO evaluation on VAL30%. CPU utilization is going very high. Pascal & Open Images V2 detection metric evaluation is going good. We choose Open Images V2 detection metric because most of the CDCA dataset is from OPEN IMAGES.
