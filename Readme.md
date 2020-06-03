To Creat Model


Part 1: Simple model

https://blog.insightdatascience.com/how-to-train-your-own-yolov3-detector-from-scratch-224d10e55de2

Note:
1:  !pip install -r requirements.txt

2:  !pip uninstall tensorflow

3:  !pip install tensorflow_gpu==1.14.0

4:  !rm /content/TrainYourOwnYOLO/Data/Model_Weights -r
    
    
    !ln -s /content/drive/'My Drive'/backup /content/TrainYourOwnYOLO/Data/Model_Weights
    
5:  !cp  /content/drive/'My Drive'/backup/trained_weights_final.h5 /content/TrainYourOwnYOLO/2_Training/src/keras_yolo3/



Part 2: Tensorflow .pb model

Step1: In 2_training folder, open "Train_YOLO.py" and change "model.save_weights" to "model.save" in line number 219 & 268

Step2: Go into "/TrainYourOwnYOLO/2_Training/src/keras_yolo3" open "pb_convert.py" and set yolo_body(Input(shape=(None, None, 3)), 3, num_classes) where num_classes is your number of classes in line number 55 & 122

Step3: Go into "/TrainYourOwnYOLO/2_Training/src/keras_yolo3/yolo3" open "model.py" and change "from ..yolo3.utils import compose" to "yolo3.utils import compose"  in line number 21

Step4: Go into "/TrainYourOwnYOLO/2_Training/src/keras_yolo3" and run "pb_convert.py"
  >>python pb_convert.py --input_model "path/to/keras/model.h5" --output_model="path/to/save/model.pb"
  
  https://www.cnblogs.com/fragrant-breeze/p/12995593.html



Part 3: tensorflow Tflite model creation

Step 1: Use the following program to get the name of layers  Note: input_arrays(1st layer) & output_arrays(last layer)

   For tensorflow 1:
         
            import tensorflow as tf
            from tensorflow.python.platform import gfile
            GRAPH_PB_PATH = '/content/model.pb'
            with tf.Session() as sess:
               print("load graph")
               with gfile.FastGFile(GRAPH_PB_PATH,'rb') as f:
                   graph_def = tf.GraphDef()
               graph_def.ParseFromString(f.read())
               sess.graph.as_default()
               tf.import_graph_def(graph_def, name='')
               graph_nodes=[n for n in graph_def.node]
               names = []
               for t in graph_nodes:
                  names.append(t.name)
               print(names)
         
   For tensorflow 2:
            
            import tensorflow as tf
            GRAPH_PB_PATH = './frozen_model.pb'
            with tf.compat.v1.Session() as sess:
               print("load graph")
               with tf.io.gfile.GFile(GRAPH_PB_PATH,'rb') as f:
                   graph_def = tf.compat.v1.GraphDef()
               graph_def.ParseFromString(f.read())
               sess.graph.as_default()
               tf.import_graph_def(graph_def, name='')
               graph_nodes=[n for n in graph_def.node]
               names = []
               for t in graph_nodes:
                  names.append(t.name)
               print(names)

Step 2: Run the following command to get the tflite model

        !tflite_convert \
          --output_file=/content/model.tflite \
          --graph_def_file=/content/model.pb \
          --input_arrays=conv2d_1/kernel \
          --output_arrays=conv2d_75/BiasAdd
