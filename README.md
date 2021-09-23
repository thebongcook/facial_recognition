# Raspberry Pi 4 Facial Recognition

This repository was originally forked from [carolinedunn/facial_recognition](https://github.com/carolinedunn/facial_recognition), so credit where it's due! She wrote up a great guide on [Tom's Hardware](https://www.tomshardware.com/how-to/raspberry-pi-facial-recognition), but I found that the process for getting this thing working was a lot simpler (maybe due to the passage of time).

## Pi Setup

You need a Raspberry Pi 4 (or CM4) with a Pi Camera or HQ camera attached and working. Look elsewhere for guides setting that up.

I'm currently running this on 32-bit Pi OS, but the process could work on 64-bit Pi OS once camera module support is fully functional there.

The dependencies required can be installed (as of September 2021) like so:

```bash
# Install OpenCV dependencies.
$ sudo apt-get install -y libatlas-base-dev

# Install OpenCV and face recognition libraries.
$ sudo pip3 install opencv-python face-recognition imutils

# I had to do this due to a numpy import error with OpenCV.
$ sudo pip3 install --force-reinstall numpy
```

You can technically set up Python libraries in other ways, including inside isolated environments local to your user account... but blasting everything with `sudo` is easy and for my particular needs, I'm dedicating one Pi to this task, and it's not even going to operate over the public Internet, or run untrusted code, so I'm okay with a global install.

## Training

Grab as many pictures as you want of a particular subject (e.g. you) and stash them inside a folder named "FirstName" inside the `dataset` folder.

For example, I would create a folder named "Jeff" and then take a number of pictures (10+) that only have my face in them—ideally without a hat and with a non-complex background—and put them inside.

Then run:

```
$ python3 train_model.py
```

This takes a couple minutes, and generates an `encodings.pickle` file that has all the relevant facial recognition information.

Note that you can have multiple people identified inside the `dataset` folder—just create a unique name for each folder. Like "Jeff", "John", "Sally", etc.

## Usage

Once the training is complete, and the `encodings.pickle` file is present, you can run the facial recognition code with:

```bash
$ python3 facial_recognition.py
```

This Python script will launch a new window after a few seconds, and start identifying faces in the visible frame.

To quit, press "q". At the end, it will output some performance statistics in the terminal. On a normally-clocked Pi 4, expect around 2 FPS performance.
