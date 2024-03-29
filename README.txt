***************************Discrete Event Simulator For RTOS Scheduler***************************

Scheduling algorithms implemented:
First In First Out (FIFO) - fifo.cpp
Rate Monotonic Scheduling (RMS) - rms.cpp
Earliest Deadline First (EDF) - edf.cpp

*************************************************************************************************

Project directory for RTOS Scheduler. RMS, EDF & FIFO scheduling with unit tests.

To run with DICE framework tests:
-Execute makeme in /src/ folder.
-Call DICE dxtest within /test/ folder.
-Output files generated.

To run independently:
-Compile C++ file (rms.cpp [RMS], edf.cpp [EDF], fifo.cpp [FIFO])
-Run: executable.out(exe) setup.txt arrivals.txt

*************************************************************************************************
Tests for the simulator.

Every Individual Test Subdirectory should include two files "setup.txt" and
"arrivals.txt" where the inputs for your simulation are defined. 

I- setup.txt:

The file setup.txt consists of three parts:
 -Part1: One line (i.e., the first line in the file) which is a positive
integer that sets the number of tasks "n_tasks" that the RTOS should schedule.

 -Part2: "n_tasks" lines that define the tasks that the system should schedule.
Every line has the format:

    <task_id>  <frequency>

  Where <task_id> is a positive non-zero integer and <frequency> is the period
of the task <task_id>.

 - Part3: One line that contains a positive integer defining the context
   switching overhead. This overhead can equal zero. This overhead should be
   simulated when the execution of a given task is interrupted by a higher
   priority task


For example, "setup "a system with 2 tasks and scheduler overhead of 1 unit
will look like:    

2
1 100
2 50 
1


II- arrivals.txt
The input file arrivals.txt consists of 2 parts 

 -Part1: One line (i.e., the first line in the file) which is a positive
integer that sets the number of events "n_events" your simulator will trace.

 -Part2: "n_events" lines that define the events the simulator will trace.
Every line has the format: 

  <time> <task_id> <execution_time> <deadline>

 where:
  <time> is a positive integer representing the time unit when the event occurs. 
  <task_id> is a positive integer that is less than or equal <n_tasks> defining the task_id for the event.  
  <execution_time> is non-zero positive integer representing the time required to execute the task that associates this event.  
  <deadline> is non-zero positive integer representing the time by which this
event should complete execution.

For example, using the setup.txt example shown above, arrivals.txt can look like:
3
0 1 20 100
1 2 10 50
60 2 10 100

III- correct-output.txt   
correct-output.txt contains the simulation trace for the system defined in
setup.txt and analysis.txt. As the simulator is implemented as a discrete
events system, we are only interested in the moment when the task being
executed changes.  

Therefore correct-output.txt will consist of multiple lines where every line
indicates a change in the task being executed by the system. 

Every line in correct-output.txt has the format:

<time> <task-id>.<task-instance>

Note: There should be a SINGLE space " " between these three fields.

<time> is the time when the simulator switches tasks. 

<task-id> is the id of task that will start execution.

<task-instance> is a non zero positive integer representing each subsequent
instance for the task "task-id". The value of "task-instance" should start by 1
, and be incremented by 1 for every task.  

Two special values of "task-id" are reserved for the context switching overhead and the
idle execution. These values are: '0' for the context switching overhead and "-1" for
idle execution.

For example, given the above values for setup.txt and arrivals.txt, using a RMS or EDF schedules,
the correct-output.txt should be:
0 1.1
1 0.1
2 2.1
12 1.1
31 -1.1
60 2.2
70 -1.2
  
*************************************************************************************************

