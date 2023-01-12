************
Installation
************

Local installation
==================

.. note::
    **Prerequisites:**
        * Ubuntu 20 (or 22)
        * Python3
        * ROS 2 (Foxy, Galactic, Humble)



Clone the repository

.. code-block:: bash

    $ cd ~/ros2_ws/src
    $ git clone --recurse-submodules https://github.com/MERLIN2-ARCH/merlin2.git
    $ cd merlin2


Install SMTPlan+ dependencies

.. code-block:: bash

    $ sudo apt install libz3-dev -y

Install unified-planning

.. code-block:: bash

    $ pip3 install --pre unified-planning[pyperplan,tamer]


Install MongoDB

.. code-block:: bash

    $ sudo ./scrips/install_mongo.sh
    $ sudo ./scrips/install_mongocxx.sh

Install sst dependencies

.. code-block:: bash

    $ sudo apt-get install -y python-dev libportaudio2 libportaudiocpp0 portaudio19-dev libasound-dev swig
    $ python3 merlin2_arch/merlin2_reactive_layer/speech_to_text/nltk_download.py

Install tts dependencies

.. code-block:: bash

    $ sudo apt install espeak -y
    $ sudo apt install speech-dispatcher -y
    $ sudo apt install festival festival-doc festvox-kdlpc16k festvox-ellpc11k festvox-italp16k festvox-itapc16k -y
    $ sudo apt install mpg321 -y


Install all python requirements

.. code-block:: bash

    $ pip3 install -r requirements.txt


Compile using colcon

.. code-block:: bash

    $ cd ~/ros2_ws
    $ colcon build


Gazebo Simulation for Demos
---------------------------

To run the demos, the `ros2_rb1 repository <https://github.com/mgonzs13/ros2_rb1.git>`_ is needed:

.. code-block:: bash

    $ cd /root/ros2_ws/src
    $ git clone -b galactic https://github.com/mgonzs13/ros2_rb1.git

    $ source /opt/ros/galactic/setup.bash
    $ cd /root/ros2_ws
    $ rosdep install --from-paths src --ignore-src -r -y

    $ source /opt/ros/galactic/setup.bash
    $ cd /root/ros2_ws
    $ colcon build


Test the installation
---------------------

.. code-block:: bash
    
    $ ./run_all_pytests.sh


Docker installation
===================

Clone the repository


Pull the Docker image from DockerHub

.. code-block:: bash

    $ docker run -p 6080:80 -p 5900:5900 -e VNC_PASSWORD=vncpassword --rm -it mgons/merlin2

or build the images from the Dockerfiles

.. code-block:: bash

    $ git clone https://github.com/MERLIN2-ARCH/merlin2_docker.git
    $ cd merlin2_docker
    $ docker build -t merlin2 ./

