CCN Application Checkpointing (CCNAC) is a plugin for DMTCP project. You can check out the DMTCP source from https://github.com/dmtcp/dmtcp

Any problems with CCNAC can be mailed to me at nitinder1369@iiitd.ac.in

UPDATE: Due to certain dependencies, several users have reported errors while checkpointing. We are hard at trying to resolve all such issues.
Please watch out for future commits.

Pre-requisites: System should have CCNx v0.8.2 installed before attempting to install CCNAC.
Several users have complained issues with CCNx v1.0. Please consider that particular version unsupported till further updates.

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
..................................................................

CCNAC WITH DMTCP RUNNING GUIDELINES AND INSTRUCTIONS

..................................................................
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

To install, use:
1. download the tar file from server and untar into directory,

2. Then
  ./configure
  make
  make check [Important if you want to compile test files as well]
  [This automatically installs CCNAC along with DMTCP]

3. make install

4. Open new window and start dmtcp coordinator
	dmtcp_coordinator
	
	[Ensure that coordinator registers itself of CCNx as new face for routing will be added]
	[Press h for help and all commands]

5. On previous window, run a simple program (pre-developed. keeps counting until explicitely killed) 

	dmtcp_checkpoint test/dmtcp1

6. While program is running, go to the coordinator and take checkpoint by typing "c"

7.  list the checkpoint in the directory, name of checkpoint can be random, use tab key to autofill

	ls -l ckpt_dmtcp_......

8. Restart from the checkpoint taken

	dmtcp_restart ckpt_dmtcp_...

Numbering will now restart from the position where checkpoint was taken.

>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
.................................................................

RUNNING CCNAC ON REMOTE NODES

..................................................................
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

1. To run DMTCP on remote nodes, get DMTCP coordinator to work on only one node:

			dmtcp_coordinator

## Note the IP address of the machine. This will be used in hostname

2. To connect to this DMTCP_Coordinator, use the command:

	dmtcp_checkpoint --host <IP address of host machine> --port <port number if specified, default:7779> --ccnURI <URI given to that particular node> test/dmtcp1 [any application that you want to run]

3. Check if the machine has connected by doing "l" on coordinator


>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
..................................................................

CHECKPOINTING ON REMOTE NODES

..................................................................
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

1. start dmtcp_coordinator on any one of the nodes. (note the host address on which the coordinator is now running)

2. In a new window, connect to localhost coordinator running dmtcp1 test.
	dmtcp_checkpoint --host 127.0.1.1 --port 7779 test/dmtcp1

3. On remote node, connect to dmtcp coordinator and run dmtcp1 as test.
	dmtcp_checkpoint --host <IP address of host machine> --port <port number if specified, default:7779> --ccnURI <ccn name of the coordinator> test/dmtcp1 

4. Take a manual checkpoint on coordinator by "c"

5. Kill the machines.

6. ls -l ckpt... on both machines.

7. restart checkpoint on Node 1 by:
	dmtcp_restart --host 127.0.1.1 --port 7779 ckpt...

==>> At this point list the nodes connected on coordinator and you would see a node joined as checkpointed. No result will be displayed until all the nodes restart from the checkpoint.

##### Even if one of the existing nodes is still running on coordinator, restart command would throw a JASSERT error. All nodes must be killed to resume from stable state.

8. restart checkpoint on Node 2 by:
	dmtcp_restart --host 192.168.32.192 --port 7779 --ccnURI <ccn name of the coordinator> ckpt....

9. If all the nodes are connected in such a way, the counting will resume from local checkpoint simultaneously.
