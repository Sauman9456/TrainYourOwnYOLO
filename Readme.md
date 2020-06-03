To Creat Model
Part 1:
https://blog.insightdatascience.com/how-to-train-your-own-yolov3-detector-from-scratch-224d10e55de2

Note:
1:  !pip install -r requirements.txt

2:  !pip uninstall tensorflow

3:  !pip install tensorflow_gpu==1.14.0

4:  !rm /content/TrainYourOwnYOLO/Data/Model_Weights -r
    
    
    !ln -s /content/drive/'My Drive'/backup /content/TrainYourOwnYOLO/Data/Model_Weights
    
5:  !cp  /content/drive/'My Drive'/backup/trained_weights_final.h5 /content/TrainYourOwnYOLO/2_Training/src/keras_yolo3/

Part:2

Step1: In 2_training folder, open "Train_YOLO.py" and change "model.save_weights" to "model.save" in line number 219 & 268

Step2: Go into "/TrainYourOwnYOLO/2_Training/src/keras_yolo3" open "pb_convert.py" and set yolo_body(Input(shape=(None, None, 3)), 3, num_classes) where num_classes is your number of classes in line number 55 & 122

Step3: Go into "/TrainYourOwnYOLO/2_Training/src/keras_yolo3/yolo3" open "model.py" and change "from ..yolo3.utils import compose" to "yolo3.utils import compose"  in line number 21

Step4: Go into "/TrainYourOwnYOLO/2_Training/src/keras_yolo3" and run "pb_convert.py"
  >>python pb_convert.py --input_model "path/to/keras/model.h5" --output_model="path/to/save/model.pb"
  
  https://www.cnblogs.com/fragrant-breeze/p/12995593.html
