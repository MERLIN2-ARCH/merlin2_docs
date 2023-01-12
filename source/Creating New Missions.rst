*********************
Creating New Missions
*********************

The creation of a new missions consists on creating the initial state,
objects and propositions, and defining the sequence of goals that the 
robot has to achieve.

To do this, the `Merlin2MissionNode class <https://github.com/MERLIN2-ARCH/merlin2/blob/b91b04bc02dfbd22b7976a42e2281620aef57eba/merlin2_arch/merlin2_mission_layer/merlin2_mission/merlin2_mission/merlin2_mission_node.py>`_ is provided.
It has three methods that has to be overrided:

* **create_objects**: this methods returns a list od PDDL objects (PddlObjectDto) that represents the objects of the initial state.
* **create_propositions**: this methods returns a list od PDDL propositions (PddlPropositionDto) that represents the propositions of the initial state.
* **execute_mission**: this method contains the sequence of goals, that are PDDL propositions that must be true. Merlin2MissionNode has the **execute_goal** method to add and execute goals.

An example of a mission is presented in the `demo <https://github.com/MERLIN2-ARCH/merlin2/blob/main/merlin2_demo/merlin2_demo/merlin2_demo_node.py>`_.
There is also a state machine version, `Merlin2FsmMissionNode <https://github.com/MERLIN2-ARCH/merlin2/blob/b91b04bc02dfbd22b7976a42e2281620aef57eba/merlin2_arch/merlin2_mission_layer/merlin2_mission/merlin2_mission/merlin2_fsm_mission_node.py>`_
, that can. The main different is that the execute_mission method is not
overrided since it runs the state machines that must be build. As a result,
the state machine contains the sequence of goals. An example of this is 
presented in the `demo2 <https://github.com/MERLIN2-ARCH/merlin2/blob/main/merlin2_demo/merlin2_demo/merlin2_demo2_node.py>`_
