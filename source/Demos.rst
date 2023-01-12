*****
Demos
*****

These demos are tested with `ros2_rb1 <https://github.com/mgonzs13/ros2_rb1>`_ simulation.

Demo 1
======

This a navigation, STT, TTS demo using the RB1 robot.

.. code-block:: bash

    $ ros2 launch rb1_gazebo gazebo_nav2.launch.py
    $ ros2 launch merlin2_demo merlin2_demo.launch.py


Demo 2
======

The RB1 robot will start driving to specific points in the world. Half of the goals are canceled randomly. Distance and time are saved in a CSV file.

.. code-block:: bash

    $ ros2 launch rb1_gazebo granny.launch.py
    $ ros2 launch merlin2_demo merlin2_demo2.launch.py

.. image:: ../images/demo2.gif
