# Aquatic ID!

Hello! I re-trained this Resnet-18 AI model  to be able to accurately identify and classify six different seas animasl. These animals were picked with the intention of aiding ocean exploration and identifying animals similar to ones already found. it is also intended to teach children nicher animals that are different to terrestrials found in farmland settings. There are two mammals (Dolphin and Otter), one cephalopod (Octopus), one pinniped (Seal), and one Asteroid (Starfish). Each animal decreases in resemblance to mammals, and increasing in uniqueness. The AI classifies accurately, and classifies animals that are not one of the 6 classses by resemblance to one of those six animals (Ex: A dog will be recognized as an otter, with a x% likelihood.) It can also identify images through a webcam (this requires you to be connected to a separate monitor to be performed due to restrictions placed upon the ORIN.) (very sad)

There was a lot of perseverance to be had while making this project. Initially, this AI was supposed to identify words and images on a page using resnet and images of each word. However, disaster struck twice - not only did my SD card have to be rebooted, but the entire project itself disappeared without a trace while it trained overnight. I did not let these roadblocks stop me, though.

[an image of the project running in VS Code with accuracy](image.png)

## The Algorithm

The algorithm works extremely simply. It is trained on ~9000 photos of the respective animals, and scans a .jpg/.jpeg image that is stored within a directory named after one of the six animals. The model running depends purely on access to directories and specific files. If a directory is off, or a file does not exist, it will not run nor classify your desired image.

## Running this project

Before you do anything, make sure you have VS Code downloaded, a Jetson Orin Nano, and access to THIS github repository: https://github.com/dusty-nv/jetson-inference, as well as the resnet18 model downloaded, a mobile hotspot, connection to your orin via mobile hotspot, NoMachine, a dummy plug for the HDMI port, and a webcam!

a. Set up your Orin - install all updates, and make sure it can continually run as long as its plugged in. There are videos online for proper set up, btu just know that your orin should be in headless mode for the duration of the model's running.

b. if not already imported, import the repository above into your orin. Everything you need should be on it if its not already ON your orin.
1. Make sure that the model has been trained using the kaggle dataset below:

import kagglehub

##Download latest version
path = kagglehub.dataset_download("ismail703/marine-animals-dataset")

print("Path to dataset files:", path)

!This is the baseline of training.
!Also, make sure that the classes 'Dolphin', 'Octopus', 'Otter', 'Sea Turtle', 'Seal', and 'Starfish' are in 3 separate test, train, and val directories.(I am aware that you should already have these from the start, but I just want to cover all bases.)
!Also (part two), open the docker container by navigating into the jetson-inference folder with cd ~/jetson-inference and opening the docker with ./docker/run.sh. Then, navigate to cd python/training/classification and run python3 train.py --model-dir=models/data data/
When the model is finished training, convert it to a python3 onnx_export.py --model-dir=models/data (onnx file).

2. Exit the docker with Ctrl + D and run cd jetson-inference/python/training/classification.

3. Use ls models/cat_dog/ to make sure that the model is there. You should see a file called resnet18.onnx.

4. Set the NET and DATASET variables: NET=models/data
DATASET=data

5. Finally, run this: imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/<animal>/<animal.jpeg/.jpg>

6. Done! You can also insert your own images into these class directories to see if the AI is able recognize it. You are not limited to the test files.

![Video explanation here!!!](demo.mp4)

Google drive link - https://drive.google.com/file/d/1QGvNMOLOXjeh3MRvT56Od2kj2H3Icoa-/view?usp=sharing
