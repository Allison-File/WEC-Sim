
This section provides an overview of WEC-Sim's mooring class features; for more 
information about the mooring class code structure, refer to 
:ref:`user-code-structure-mooring-class`. 

Floating WEC systems are often connected to mooring lines to keep the device in 
position. WEC-Sim allows the user to model the mooring dynamics in the 
simulation by specifying the mooring matrix or coupling with MoorDyn. To 
include mooring connections, the user can use the mooring block (i.e., Mooring 
Matrix block or MoorDyn block) given in the WEC-Sim library under Moorings lib 
and connect it between the body and the Global reference frame. Refer to the 
:ref:`rm3_moordyn`, and the :ref:`webinar4` for more information. 

MoorDyn is hosted on a separate `MoorDyn repository <https://github.com/WEC-Sim/moorDyn>`_. 
It must be download separately, and all files and folders should be placed in 
the ``$WECSIM/functions/moorDyn`` directory. 

.. _mooring-matrix:

Mooring Matrix
^^^^^^^^^^^^^^

When the mooring matrix block is used, the user first needs to initiate the 
mooring class by setting :code:`mooring(i) = mooringClass('mooring name')` in 
the WEC-Sim input file (``wecSimInputFile.m``). Typically, the mooring 
connection location also needs to be specified, :code:`mooring(i).location = [1x3]` 
(the default connection location is ``[0 0 0]``). The user can also define the 
mooring matrix properties in the WEC-Sim input file using: 

* Mooring stiffness matrix - :code:`mooring(i).matrix.stiffness = [6x6]` in [N/m]

* Mooring damping matrix - :code:`mooring(i).matrix.damping = [6x6]` in [Ns/m]

* Mooring pretension - :code:`mooring(i).matrix.preTension = [1x6]` in [N]

.. Note::

    "i" indicates the mooring number. More than one mooring can be specified in 
    the WEC-Sim model when the mooring matrix block is used. 

.. _mooring-lookup:

Mooring Lookup Table
^^^^^^^^^^^^^^^^^^^^

When the mooring lookup table block is used, the user first needs to initiate the 
mooring class by setting :code:`mooring(i) = mooringClass('mooring name')` in 
the WEC-Sim input file (``wecSimInputFile.m``). Typically, the mooring 
connection location also needs to be specified, :code:`mooring(i).location = [1x3]` 
(the default connection location is ``[0 0 0]``). The user must also define the 
lookup table file in the WEC-Sim input file with :code:`mooring(i).lookupTableFile = 'FILENAME';`

The lookup table dataset should contain one structure that contains fields for each index and each force table:


	+----------------+----------------------+--------------+
	| *Index Name*   |    *Description*     | *Dimensions* |
	+----------------+----------------------+--------------+
	|       X        | Surge position [m]   |    1 x nX    |
	+----------------+----------------------+--------------+
	|       Y        | Sway position [m]    |    1 x nY    |
	+----------------+----------------------+--------------+
	|       Z        | Heave position [m]   |    1 x nZ    |
	+----------------+----------------------+--------------+
	|       RX       | Roll position [deg]  |    1 x nRX   |
	+----------------+----------------------+--------------+
	|       RY       | Pitch position [deg] |    1 x nRY   |
	+----------------+----------------------+--------------+
	|       RZ       | Yaw position [deg]   |    1 x nRZ   |
	+----------------+----------------------+--------------+
    
    
	+----------------+--------------------+--------------------------------+
	| *Force Name*   | *Description*      |          *Dimensions*          |
	+----------------+--------------------+--------------------------------+
	|       FX       | Surge force [N]    | nX x nY x nZ x nRX x nRY x nRZ |
	+----------------+--------------------+--------------------------------+
	|       FY       | Sway force [N]     | nX x nY x nZ x nRX x nRY x nRZ |
	+----------------+--------------------+--------------------------------+
	|       FZ       | Heave force [N]    | nX x nY x nZ x nRX x nRY x nRZ |
	+----------------+--------------------+--------------------------------+
	|       MX       | Roll force [Nm]    | nX x nY x nZ x nRX x nRY x nRZ |
	+----------------+--------------------+--------------------------------+
	|       MY       | Pitch force [Nm]   | nX x nY x nZ x nRX x nRY x nRZ |
	+----------------+--------------------+--------------------------------+
	|       MZ       | Yaw force [Nm]     | nX x nY x nZ x nRX x nRY x nRZ |
	+----------------+--------------------+--------------------------------+


.. _mooring-moordyn:

MoorDyn
^^^^^^^

When the MoorDyn block is used, the user needs to initiate the mooring class by 
setting :code:`mooring = mooringClass('mooring name')` in the WEC-Sim input 
file (wecSimInputFile.m), followed by the number of mooring lines that are 
defined in MoorDyn (``mooring(1).moorDynLines = <Number of mooring lines>``) 

A mooring folder that includes a moorDyn input file (``lines.txt``) is required 
in the simulation folder. 

.. Note::
    WEC-Sim/MoorDyn coupling only allows one mooring configuration in the 
    simulation.

.. _rm3_moordyn:

RM3 with MoorDyn
""""""""""""""""

This section describes how to simulate a mooring connected WEC system in 
WEC-Sim using MoorDyn. The RM3 two-body floating point absorber is connected to 
a three-point catenary mooring system with an angle of 120 between the lines in 
this example case. The RM3 with MoorDyn folder is located under the `WEC-Sim 
Applications <https://github.com/WEC-Sim/WEC-Sim_Applications>`_ repository. 

* **WEC-Sim Simulink Model**: Start out by following the instructions on how to 
  model the :ref:`user-tutorials-rm3`. To couple WEC-Sim with MoorDyn, the 
  MoorDyn Block is added in parallel to the constraint block

.. _WECSimmoorDyn:

.. figure:: /_static/images/WECSimMoorDyn.png
    :width: 320pt
    :align: center

* **WEC-Sim Input File**: In the ``wecSimInputFile.m`` file, the user needs to 
  initiate the mooring class and define the number of mooring lines.

.. _WECSimInputMoorDyn:

.. rli:: https://raw.githubusercontent.com/WEC-Sim/WEC-Sim_Applications/master/Mooring/MoorDyn/wecSimInputFile.m
   :language: matlab

* **MoorDyn Input File**: A mooring folder that includes a moorDyn input file 
  (``lines.txt``) is created. The moorDyn input file (``lines.txt``) is shown 
  in the figure below. More details on how to setup the MooDyn input file are 
  described in the MoorDyn User Guide :cite:`Hall2015MoorDynGuide`.

.. _moorDynInput:

.. figure:: /_static/images/moorDynInput.png
    :width: 400pt
    :align: center

* **Simulation and Post-processing**: Simulation and post-processing are the 
  same process as described in Tutorial Section.

.. Note::
    You may need to install the MinGW-w64 compiler to run this simulation.
