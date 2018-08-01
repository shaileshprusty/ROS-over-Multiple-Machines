# ROS over Multiple Machines

This manual explains the process for connecting ROS Nodes on multiple machines connected to the same network.

## Example case for 2 Computers on same network:

| Hostname | comp1 | comp2 |
|:----------:|:-------:|:-------:|
| IP | 192.168.77.11 | 192.168.77.12 |

### Case 1: Using On-board ROS :-

- **Configuring ROS master**: There should be only a single **master/ROS core** for handling the system. In this example, let the _comp1_ be the master. Hence, `roscore` is run on this computer.

  `comp1: $ roscore`

- This should start the `roscore` displaying the master's IP address and the port number. `11311` is the default port of the master.

- **Configuring Slaves:**

  - The slave machines need to know the master's as well as their own IP address.

  - _comp1_ can act as a slave to itself along with being the master.

    _Specify ROS master's IP:_   
    `comp1: $ export ROS_MASTER_URI=http://192.168.77.11:11311`

    _Specify slave machine's own IP:_   
    `comp1: $ export ROS_IP=192.168.77.11`

  - _comp2_ will only act as a slave machine.
  
    _Specify ROS master's IP:_   
    `comp2 : export ROS_MASTER_URI=http://192.168.77.11:11311`

    _Specify slave machine's own IP:_   
    `comp2 : $ export ROS_IP=192.168.77.12`

  **_Note:_** In case of more number of machines, one of them can solely act as the master with the `roscore` running on it and all other nodes running on the slaves.

- **Resolve master's name to IP (Optional):**

  Modify the `/etc/hosts` file in order to resolve master's name to IP in the slave machine.
  
  - In the slave machine, _comp2_, open the file as `Root` user:
  `comp2 : $ sudo nano /etc/hosts`

  - Add the line to the `hosts` file in the format of:

    <Master's IP Address>&ensp;&ensp;&ensp;&ensp;<Master's Name>

    For example,

    <!-- <pre><code>192.168.77.11         comp1</code></pre> -->
    192.168.77.11&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;comp1

  - Save and close the file.

**_Note:_** The commands for specifying the IP address of ROS master and the slave can also be added to a bash file and sourced when required instead of typing it every time.

### Case 2: Using MATLAB-ROS:

Robotics System Toolbox enables one to interface with ROS and use ROS functionality in MATLAB and Simulink. One can connect to a ROS network, collect data, send and receive one's own messages, and deploy code to a standalone system.

- **Configuring ROS Master:**

  - To initialize ROS master using MATLAB-ROS, run the following command in the **MATLAB Command Window**:

    _For comp1,_   
    `>>   rosinit`

  - This should start the `roscore` and display the IP address of ROS master and the port numbers of ROS Master and the global node on the command window.
  
  **_Note:_** For running a ROS node in the master, the MATLAB code just needs to be run. No separate configurations are required.

- **Configuring Slaves:**
  
  - Before running the slave nodes, MATLAB-ROS must be initialized with a master in the network. To do so, the IP address of the ROS master must be passed as an argument to the `rosinit` function.
  
    Eg: `rosinit('<Master's IP Address>')`

    _For comp2,_   
    `>>   rosinit('192.168.77.11')`

**_Note:_** In case of one of the computer with MATLAB-ROS and other with On-Board ROS, the nodes can communicate over the network using the corresponding configurations.