# ImageNetPreprocess

This repo aims at recording the instructions used to preprocess imagenet dataset(ILSVRC2012).

More specifically, the imagenet dataset is compressed into several files. Extracting them to a well-organized structure requires tedious work with shell scripts. Moreover, since I'm familiar with TensorFlow, I'd like to pack the dataset into TFRecords so as to achieve high-speed data access. Therefore, I'll list the instructions to extracting the imagenet dataset into a well-organized structure.

Note that you still need to download the imagenet dataset from official site (which need register and send an formal request)

# Step 1.

Get a clone of the tensorflow's model repo from [here](https://github.com/tensorflow/models)

# Step 2.

1. Choose a directory where you want to put your data into, just like
>/home/mxf/data/ILSVRC2012

2. Make some directories which you'll need later:
> /home/mxf/data/ILSVRC2012/raw-data
> /home/mxf/data/ILSVRC2012/raw-data/bounding_boxes

3. Make some soft links to the downloaded files

>ln -s /home/mxf/data/downloaded/Annotation.tar.gz /home/mxf/data/ILSVRC2012/raw-data/bounding_boxes/annotations.tar.gz

>ln -s /home/mxf/data/downloaded/ILSVRC2012_img_train.tar /home/mxf/data/ILSVRC2012/ILSVRC2012_img_train.tar

>ln -s /home/mxf/data/downloaded/ILSVRC2012_img_val.tar /home/mxf/data/ILSVRC2012/ILSVRC2012_img_val.tar

4. Enter the directory models/research/inception/inception/data, then make some modifications (turn off downloading)
  4.1. Open download_and_preprocess_imagenet.sh, comment line 61:
  > WORK_DIR="$0.runfiles/inception/inception"

  4.2. Add a line below:
  > WORK_DIR=".."

  4.3. Change line
  >BUILD_SCRIPT="${WORK_DIR}/build_imagenet_data"
  to
  >BUILD_SCRIPT="${WORK_DIR}/build_imagenet_data.py"

  4.3. Open download_imagenet.sh, commetn lines starting with 'wget'

5. Execute download_and_preprocess_imagenet.sh with specified dir
> download_and_preprocess_imagenet.sh /home/mxf/data/ILSVRC2012

