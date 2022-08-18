# Venomous Snake Avoider

My project's purpose is to help people who like to hike, specifically in California, avoid snakes that have venom that are harmful for humans. The machine uses the Resnet-18 network, which is retrained with images of the most common venomous and nonvenomous snakes in California, to detect and tell the person whether the snake shown in the image is venomous or nonvenomous.

![add image descrition here](direct image link here)

## The Algorithm

My project uses image classification to tell which of California's most common snakes are venomous, and which are nonvenomous. The images are from a custom dataset. It uses py.torch to train the data. After putting an image through the ResNet.18 network, it will add the classification and the percent confidence the computer has that the classification is correct on top of the image. You will then be able to view the image with the classification and confidence percentage through the host computer using scp. 

## Materials

1. computer
2. Jetson Nano
3. USB to microUSB Cable (to connect Nano to computer)
4. USBC power supply
5. ethernet or wifi connection straight to nano

## Running this project

1. Start by plugging micro usb, usbc power cord, and ethernet cable to the nano.
2. ssh into the nano by running the command ``ssh <username>@192.168.55.1`` and entering your password for your nano
3. Run the following commands to ensure git and cmake are installed. It should ask you to enter your password.

``
sudo apt-get update
``
``
sudo apt-get install git cmake
``

4. Run this command to clone the jetson-inference project

``git clone --recursive https://github.com/dusty-nv/jetson-inference``

``cd jetson-inference``

``git submodule update --init``

5. To install the python packages, run the following command

``sudo apt-get install libpython3-dev python3-numpy``

6. Next, you have to make a build directory where your project will be. Make sure you are in the **jetson-inference** directory by running ``cd jetson-inference`` then run:

``mkdir build``

7. Change into the newly made build directory by running ``cd build`` then run the following command:

``cmake ../``

8. While configuring, you will see a downloader tool. For this project, make sure you are downloading Resnet-18, then click OK to continue. 

![image](https://user-images.githubusercontent.com/110637561/184810596-5d2e7d43-4fb6-4adf-b85c-2499bfa4fba6.jpeg)

9. Afterwards, the PyTorch installer will appear. Since there is no need to install PyTorch, make sure there are no stars in the installation options. If there is, use the spacebar to un-select it, then press enter to continue. image

![image](https://user-images.githubusercontent.com/110637561/184810776-48cb9746-f720-48e1-ae8e-0872fe6ea360.jpeg)

10. Once this finishes loading, it should show you the space to enter commands and allow you to run them again. Ensure you are still in the build directory then run the following commands:

``make``

``sudo make install``

``sudo ldconfig``

11. Once this is finished, you will be able to run the snake project correctly. 
12.Download this dataset that holds all the snake pictures (located in repository) and transfer it from your host computer to your nano using the command below. Another simplier way to transfer the file into your nano is by using Visual Studio Code and by dragging and dropping it into the jetson-inference/python/training/classification/data directory.

``scp venomous_nonvenomous <nanousername>@192.168.55.1:/home/jetson-inference/python/traing/classification/data``

13. Change directories into **jetson-inference/python/training/classification/data** to check if the venomous_nonvenomous file is there.
14. Go back to to the **jetson-inference** directory and run ``./docker/run.sh`` to run the docker container.
15. Run the training script to re-train the network. The model-dir arguement is where the model should be saved and where the data is. You should immediately start to see output, but it will take a long time to finish running.

``python3 train.py --model-dir=models/venomous_nonvenomous data/venomous_nonvenomous``

You can stop the running at any time using ``Ctl+C``. Try to wait at least 5 epochs before stopping the running. 

16. The ResNet-18 model is now retrained and the next step is to convert it into ONNX format. This model simplifies the process of sharing models between tools. PyTorch comes with built-in support, so follow the next steps to export your model. Start by making sure you're in the docker container and in the **jetson-inference/python/training/classification directory**.

17. Run the ONNX export script

''python3 onnx_export.py --model-dir=models/venomous_nonvenomous''

18. Look in **jetson-inference/python/training/classification/venomous_nonvenomous** to check if the new re-trained model called resnet19.onnx is there.

19. Next, you can start processing images through your re-trained network. Exit the docker container by pressing ``Ctl+D``.

20. On your nano, make sure you are in the **jetson-inference/python/training/classification directory**.

21. Use ``ls models/venomous_nonvenomous`` to make sure the resnet18.onnx file, which is the model, is on the nano.

22. Set the NET and DATASET variables

``NET=models/venomous_nonvenomous``

``DATASET=data/venomous_nonvenomous``

23. Run this command to see how it operates on an image (specifically of a california mountain king snake) from the nonvenomous snake folder:

``imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/nonvenomous/calimountkingtest1.jpeg nonvenomous.jpg``

24. Use scp to view the image on your host computer:

If you're on Windows, use: ``scp <nanousername>@192.168.55.1:/home/<nanousername>/jetson-inference/python/training/classification/nonvenomous.jpg C:\Users\<hostusername>\Desktop``

If you're on Mac, use: ``scp <nanousername>@192.168.55.1:/home/<nanousername>/jetson-inference/python/training/classification/nonvenomous.jpg ./``

The image should show up looking like this:

![nonvenomous](https://user-images.githubusercontent.com/110637561/184809863-aa2198ee-4fed-437c-b338-562208171e01.jpg)


That's it!

## Video Demonstration

https://youtu.be/k3e_IP2KVMc 
