# Self Driving Car  

This repo contains all the code for the self-driving RC car that I am making as part of the
[#100DaysOfMLCode](https://twitter.com/sirajraval/status/1014758160572141568) challenge. Follow
my progress at [my personal website](https://sagnibak.github.io/) where I am logging milestones
of this project and also presenting all the information with enough detail to allow anyone to
follow along if they want.

## CARLA Simulator Scripts

The directory `CARLA_simulator_scripts` currently contains the scripts I used to collect camera, depth, and
semantic segmentation ground truth data, but you will need the [CARLA simulator](http://carla.org),
specifically [version 0.8.4](http://carla.org/2018/06/18/release-0.8.4/) in order to run those scripts.

First, you need to download the prebuilt binaries for Windows or Linux from the releases page
[here](https://github.com/carla-simulator/carla/releases/tag/0.8.4) and extract the files in the archive.
In it you will find the simulator and a directory called `PythonClient`. You need to move my scripts into
the `PythonClient` directory. Then open two terminals. In one of the terminals, run the following in order
to start the simulator in server mode (based on your OS):

```bash
# on Linux
./CarlaUE4.sh -carla-server -benchmark -fps=20

# on Windows
CarlaUE4.exe -carla-server -benchmark -fps=20
```

The `-carla-server` flag tells the simulator to run as a server and expect Python clients to connect. The
`-benchmark` flag instructs the simulator to run in
[fixed time-step mode](https://carla.readthedocs.io/en/0.8.4/configuring_the_simulation/#fixed-time-step)
and the `-fps` flag specifies the framerate to run the simulator at. You want to specify a framerate that
your hardware will be able to maintain. This is important because we need to run the simulator in
[synchronous mode](https://carla.readthedocs.io/en/0.8.4/configuring_the_simulation/#synchronous-vs-asynchronous-mode)
while collecting data, otherwise there is a chance that the data captured by the different sensors will
not be synchronized (this will happen if the Python client cannot keep up with the simulation, which is
often the case).

In the other terminal, run the following to start the Python client that will connect to the simulator
server to save the data in the directory `PythonClient/_out`:

```bash
cd PythonClient
python3 manual_control_rgb_semseg.py --images-to-disk --location=_out
```

Everything might take about 20-30 seconds to start up depending on your hardware, but eventually the CARLA
simulator will be launched and a black Pygame window will start. You need to keep the Pygame window in focus
and drive the car manually using the WASD keys. Once you have collected as much data as you want, simply kill
the python process and the simulator. In no time you will find a few gigabytes of Numpy arrays (`.npy` files)
in the `PythonClient/_out` directory. If you are wondering why I chose to save the data as Numpy arrays
instead of images, then read [the accompanying blogpost]().

You can view the collected data by running the Jupyter Notebook `verify_collected_data.ipynb`. This should also
be run inside the `PythonClient` directory (or you can modify the code inside the notebook to load saved
`.npy` files from whatever location you saved your data to). The notebook is self-explanatory.
