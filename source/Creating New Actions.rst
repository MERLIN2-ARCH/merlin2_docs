********************
Creating New Actions
********************

The creation of a new action is presented in this section. 
This way, navigation action is presented in PDDL, MERLIN2 and 
MERLIN2 state machine.


PDDL Example
============

This PDDL example shows a durative action that moves a robot from an origin (o) to a destination (d). It has two parameters o and d of type wp (waypoint), one condition, which is that the robot has to be at the origin, and two effects, which are that the robot is not at the origin but is at the destination.

.. code-block:: pddl

  (:durative-action navigation
    :parameters (?o ?d - wp)
    :duration (= ?duration 10)
    :condition (and
      (at start (robot_at ?o))
    )
    :effect (and
      (at start (not (robot_at ?o)))
      (at end (robot_at ?d))
    )
  )


Basic Example
===============

This `basic example <https://github.com/MERLIN2-ARCH/merlin2/blob/main/merlin2_arch/merlin2_executive_layer/merlin2_basic_actions/merlin2_basic_actions/merlin2_navigation_action.py>`_
presents the same PDDL durative action as the previous PDDL version but using MERLIN2. 
There are 5 methods to override:

* **run_action**: this callback is used to execute the code of the action. The PlanAction goal arg is the action planned by the planner (name of the PDDL action + PDDL objects).
* **cancel_action**: this callback is used to cancel the action execution.
* **create_parameters**: this method is used to return the list of parameters of the action (PddlObjectDto).
* **create_conditions**: this method is used to return the list of conditions of the action (PddlConditionEffectDto).
* **create_effects**: this method is used to return the list of effects of the action (PddlConditionEffectDto).


.. code-block:: python

  from typing import List
  import rclpy

  from kant_dto import (
      PddlObjectDto,
      PddlConditionEffectDto,
  )

  from merlin2_basic_actions.merlin2_basic_types import wp_type
  from merlin2_basic_actions.merlin2_basic_predicates import robot_at

  from merlin2_action.merlin2_action import Merlin2Action

  from waypoint_navigation_interfaces.action import NavigateToWp
  from merlin2_arch_interfaces.msg import PlanAction


  class Merlin2NavigationAction(Merlin2Action):

      def __init__(self):

          # create PDDL parameters as PddlObjectDto
          self.__org = PddlObjectDto(wp_type, "o")
          self.__dst = PddlObjectDto(wp_type, "d")

          # super init
          super().__init__("navigation")

          # ROS 2 interfaces
          self.__wp_nav_client = self.create_action_client(
              NavigateToWp, "/waypoint_navigation/navigate_to_wp")

      # override the run callback
      def run_action(self, goal: PlanAction) -> bool:
          nav_goal = NavigateToWp.Goal()

          dst = goal.objects[1]
          nav_goal.wp_id = dst

          self.__wp_nav_client.wait_for_server()
          self.__wp_nav_client.send_goal(nav_goal)
          self.__wp_nav_client.wait_for_result()

          if self.__wp_nav_client.is_succeeded():
              return True

          else:
              return False

      # override cancel callback
      def cancel_action(self):
          self.__wp_nav_client.cancel_goal()

      # add PDDL parameters
      def create_parameters(self) -> List[PddlObjectDto]:
          return [self.__org, self.__dst]

      # add PDDL action conditions as PddlConditionEffectDto
      def create_conditions(self) -> List[PddlConditionEffectDto]:
          condition_1 = PddlConditionEffectDto(robot_at,
                                              [self.__org],
                                              time=PddlConditionEffectDto.AT_START)
          return [condition_1]

      # add PDDL action effects as PddlConditionEffectDto
      def create_efects(self) -> List[PddlConditionEffectDto]:
          effect_1 = PddlConditionEffectDto(robot_at,
                                            [self.__dst],
                                            time=PddlConditionEffectDto.AT_END)

          effect_2 = PddlConditionEffectDto(robot_at,
                                            [self.__org],
                                            is_negative=True,
                                            time=PddlConditionEffectDto.AT_START)

          return [effect_1, effect_2]


  def main(args=None):
      rclpy.init(args=args)
      node = Merlin2NavigationAction()
      node.join_spin()
      rclpy.shutdown()

  if __name__ == "__main__":
      main()


State Machine Example
=====================

This `state machine example <https://github.com/MERLIN2-ARCH/merlin2/blob/main/merlin2_arch/merlin2_executive_layer/merlin2_basic_actions/merlin2_basic_actions/merlin2_navigation_fsm_action.py>`_ 
presents the same PDDL durative action as the previous one but using state machines. 
In this version, the action is built using states. 
run_action and cancel_action methods are not necessary because the execution depends on the execution of the state machine. 
This means that run_action executes the state machine and cancel_action stops the state machines, stopping the current state, transparently for the user.
Besides, run_action write the PlanAction goal into the blackboard. 
As a result, the goal can be used in the states of the state machine.

There are some basics states that can be accessed from `Merlin2BasicStates <https://github.com/MERLIN2-ARCH/merlin2/blob/main/merlin2_arch/merlin2_executive_layer/merlin2_fsm_action/merlin2_fsm_action/merlin2_state_factory/merlin2_basic_states.py>`_
, but new ones can be implemented using the state classes from YASMIN. The basic states are:

* NAVIGATION
* TTS
* STT

.. code-block:: python

  from typing import List
  import rclpy

  from kant_dto import (
      PddlObjectDto,
      PddlConditionEffectDto,
  )

  from merlin2_basic_actions.merlin2_basic_types import wp_type
  from merlin2_basic_actions.merlin2_basic_predicates import robot_at

  from merlin2_fsm_action import (
      Merlin2FsmAction,
      Merlin2BasicStates
  )
  from yasmin import CbState
  from yasmin.blackboard import Blackboard


  class Merlin2NavigationFsmAction(Merlin2FsmAction):

      def __init__(self):

          # create PDDL parameters as PddlObjectDto
          self.__org = PddlObjectDto(wp_type, "o")
          self.__dst = PddlObjectDto(wp_type, "d")

          # super init
          super().__init__("navigation")

          # YASMIN CbState to create the navigation goal
          prepare_goal_state = CbState(["valid"], self.prepapre_goal)

          # YASMIN state for navigation
          navigation_state = self.create_state(Merlin2BasicStates.NAVIGATION)

          # create state machine adding states
          self.add_state(
              "PREPARING_GOAL",
              prepare_goal_state,
              {"valid": "NAVIGATING"}
          )

          self.add_state(
              "NAVIGATING",
              navigation_state
          )

      # callback for YASMIN CbState
      def prepapre_goal(self, blackboard: Blackboard) -> str:
          blackboard.destination = blackboard.merlin2_action_goal.objects[1]
          return "valid"

      # add PDDL parameters
      def create_parameters(self) -> List[PddlObjectDto]:
          return [self.__org, self.__dst]

      # add PDDL action conditions as PddlConditionEffectDto
      def create_conditions(self) -> List[PddlConditionEffectDto]:
          condition_1 = PddlConditionEffectDto(robot_at,
                                              [self.__org],
                                              time=PddlConditionEffectDto.AT_START)
          return [condition_1]

      # add PDDL action effects as PddlConditionEffectDto
      def create_efects(self) -> List[PddlConditionEffectDto]:
          effect_1 = PddlConditionEffectDto(robot_at,
                                            [self.__dst],
                                            time=PddlConditionEffectDto.AT_END)

          effect_2 = PddlConditionEffectDto(robot_at,
                                            [self.__org],
                                            is_negative=True,
                                            time=PddlConditionEffectDto.AT_START)

          return [effect_1, effect_2]


  def main(args=None):
      rclpy.init(args=args)
      node = Merlin2NavigationFsmAction()
      node.join_spin()
      rclpy.shutdown()


  if __name__ == "__main__":
      main()
