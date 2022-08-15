Venomous Snake Avoider

My project's purpose is to help people who like to hike, specifically in California, avoid snakes that have venom that are harmful for humans. The machine uses the Resnet-18 network, which is retrained with images of the most common venomous and nonvenomous snakes in California, to detect and tell the person whether the snake shown in the image is venomous or nonvenomous. 

![add image descrition here](direct image link here)

## The Algorithm

Add an explanation of the algorithm and how it works. Make sure to include details about how the code works, what it depends on, and any other relevant info. Add images or other descriptions for your project here. 

## Materials

1. computer
2. Jetson Nano
3. USB to microUSB Cable (to connect Nano to computer)
4. USBC power supply
5. ethernet or wifi connection straight to nano

## Running this project

1. Start by plugging micro usb, usbc power cord, and ethernet cable to the nano.
2. ssh into the nano by running the command "ssh <username>@192.168.55.1" and entering your password for your nano
3. Run the following commands to ensure git and cmake are installed. It should ask you to enter your password.
sudo apt-get update
sudo apt-get install git cmake
4. Run this command to clone the jetson-inference project
git clone --recursive https://github.com/dusty-nv/jetson-inference
cd jetson-inference
git submodule update --init
5. To install the python packages, run the following command
sudo apt-get install libpython3-dev python3-numpy
6. Next, you have to make a build directory where your project will be. Make sure you are in the jetson-inference directory by running cd jetson-inference then run:
mkdir build
7. Change into the newly made build directory by running "cd build" then run the following command:
cmake ../
8. While configuring, you will see a downloader tool. For this project, make sure you are downloading Resnet-18, then click OK to continue.
![image](https://user-images.githubusercontent.com/110637561/184583246-526388bd-64c4-4c21-86ac-a58dd418942f.png)
9. Afterwards, the PyTorch installer will appear. Since there is no need to install PyTorch, make sure there ar4 no stars in the installation options. If there is, use the spacebar to un-select it, then press enter to continue. 
![image](https://user-images.githubusercontent.com/110637561/184583432-19326f23-d91d-4407-8d36-742eb6849753.png)
10. Once this finishes loading, it should show you the space to enter commands and allow you to run them again. Ensure you are still in the build directory then run the following commands: 
make
sudo make install
sudo ldconfig
11. Once this is finished, you will be able to run the snake project correctly. 



[View a video explanation here](video link)
