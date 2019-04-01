# ImageNetPreprocess

This repo aims at recording the instructions used to preprocess imagenet dataset(ILSVRC2012).

More specifically, the imagenet dataset is compressed into several files. Extracting them to a well-organized structure requires tedious work with shell scripts. Moreover, since I'm familiar with TensorFlow, I'd like to pack the dataset into TFRecords so as to achieve high-speed data access. Therefore, I'll list the instructions to extracting the imagenet dataset into a well-organized structure.

Note that you still need to download the imagenet dataset from official site (which need register and send an formal request)

# Step 1. Clone the tensorflow's model zoo

Get a clone of the tensorflow's model repo from [here](https://github.com/tensorflow/models)

# Step 2. Prepare data

1. Choose a directory where you want to put your data into, just like

>/home/mxf/data/ILSVRC2012

2. Make some directories which you'll need later:

> /home/mxf/data/ILSVRC2012/raw-data

> /home/mxf/data/ILSVRC2012/raw-data/bounding_boxes

3. Make some soft links to the downloaded files

>ln -s /home/mxf/data/downloaded/Annotation.tar.gz /home/mxf/data/ILSVRC2012/raw-data/bounding_boxes/annotations.tar.gz

>ln -s /home/mxf/data/downloaded/ILSVRC2012_img_train.tar /home/mxf/data/ILSVRC2012/ILSVRC2012_img_train.tar

>ln -s /home/mxf/data/downloaded/ILSVRC2012_img_val.tar /home/mxf/data/ILSVRC2012/ILSVRC2012_img_val.tar

4. Extract Annotation.tar.gz to a folder, then you'll see many zipped (\*.tar.gz) files. Extract all the small zip files. You can refer to the scripts as follows:
```
mkdir Annotation
tar xf Annotation.tar.gz -C Annotation/
for SYNSET in Annotation/*.tar.gz
do
    echo "Processing: Annotation/${SYNSET}"
    tar xf ${SYNSET}
    rm ${SYNSET}
done

echo "End"
```

# Step 3. Adjust preprocess scripts
1. Enter the directory models/research/inception/inception/data, then make some modifications (turn off downloading)
  1.1. Open download_and_preprocess_imagenet.sh, change line
  
  > WORK_DIR="$0.runfiles/inception/inception"
  
  to
  
  > WORK_DIR=".."

  1.2. Change line
  
  >BUILD_SCRIPT="${WORK_DIR}/build_imagenet_data"
  
  to
  
  >BUILD_SCRIPT="${WORK_DIR}/build_imagenet_data.py"

  1.3. Open download_imagenet.sh, commetn lines starting with 'wget'
  
  1.4 Open build_imagenet_data.py and change the first line to your customized python path, such as
  
  > #!/home/mengxiangfei/home/miniconda3/envs/tf/bin/python

2. Execute download_and_preprocess_imagenet.sh with specified dir

> download_and_preprocess_imagenet.sh /home/mxf/data/ILSVRC2012
