************************************
Welcome to MERLIN2's documentation!
************************************

`MERLIN 2 (MachinEd Ros pLanINg) <https://github.com/MERLIN2-ARCH/merlin2>`_ 
is a cognitive architecture for robots based on ROS 2. It is aimed to be used 
to generate behaviors in robots using PDDL planners and state machines from
`YASMIN (Yet Another State MachINe) <https://github.com/uleroboticsgroup/yasmin>`_ .
PDDL can be manageg using `KANT (Knowledge mAnagemeNT) <https://github.com/uleroboticsgroup/kant>`_.


Citations
=========

.. code-block:: bibtex
   @article{GONZALEZSANTMARTA2023100477,
      title = {MERLIN2: MachinEd Ros 2 pLanINg},
      journal = {Software Impacts},
      volume = {15},
      pages = {100477},
      year = {2023},
      issn = {2665-9638},
      doi = {https://doi.org/10.1016/j.simpa.2023.100477},
      url = {https://www.sciencedirect.com/science/article/pii/S2665963823000143},
      author = {Miguel Á. González-Santmarta and Francisco J. Rodríguez-Lera and Camino Fernández-Llamas and Vicente Matellán-Olivera},
      keywords = {Cognitive robotics, Hybrid architecture, Symbolic planning, Finite state-machines, Reactivity, Knowledge managing},
      abstract = {Any service robot should be able to make decisions and schedule tasks to reach predefined goals such as opening a door or assisting users at home. However, these processes are not single short-term tasks anymore and it is required to set long-term skills for establishing a control architecture that allows robots to perform daily tasks. This paper presents MERLIN2, a hybrid cognitive architecture based on symbolic planning and state machine decision-making systems that allows performing robot behaviors. The architecture can run in any robot running ROS 2, the latest version of the Robot Operative System. MERLIN2 is available at https://github.com/MERLIN2-ARCH/merlin2.}
   }

.. code-block:: bibtex

   @article{Gonz_lez_Santamarta_2020,
      doi = {10.3390/app10175989},
      url = {https://doi.org/10.3390%2Fapp10175989},
      year = 2020,
      month = {aug},
      publisher = {{MDPI} {AG}},
      volume = {10},
      number = {17},
      pages = {5989},
      author = {Miguel {\'{A}}. Gonz{\'{a}}lez-Santamarta and Francisco J. Rodr{\'{\i}}guez-Lera and Claudia {\'{A}}lvarez-Aparicio and {\'{A}}ngel M. Guerrero-Higueras and Camino Fern{\'{a}}ndez-Llamas},
      title = {{MERLIN} a Cognitive Architecture for Service Robots},
      journal = {Applied Sciences}
   }


Contents
==================

.. toctree::
   :maxdepth: 2

   Architecture
   Installation
   PDDL Planners
   Creating New Actions
   Creating New Missions
   Demos
   Documentation