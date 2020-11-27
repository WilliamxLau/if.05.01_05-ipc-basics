
# 1) What is a race condition?

If there are two threads that are running the same code at the same time want to access a ressource that is shared, 
they will probably override variables which leads to data loss


# 2) Disabling Interrupts
  

i.  Interrupts can only be disabled on the current core, but other cores are stil able to access the shared resources 

ii. If interrupts aren't reenabled, the computer wouldn't be able to react to other interrupts

# 3) Peterson's Solution

## 1. Scenario
Process 0 left the enter_region() because it was the faster one.
Process 1 is waiting in the enter_region() because it was the slower process.
Process 0 is granted access to the shared data.
Process 0 hasfinished and leaves the critical reagion with 
leave_reagion().
Process 1 left the enter_region() and is granted access to the shared data.

## 2. Scenario
Process 0 left the enter_region() and process 1 is entering right after.
Both values of the indexers in the interesteed array are set to true.
The loser variable is set to 0 which calls Process 0 into the loop, after that the loser variable is set to 1.
Process 1 leaves the loop and the method because the loser variable isn´t 0 anymore.
The now loser variable is set to 1 which calls the Process 1 into the loop.
When Process 0 calls leave_region() and the interested[0] is set to false, process 1 is able to leave the loop.

ii. 
Success isnt guaranteed for this if the first process doesn´t even enter the cirtical region. 
As long as the lock variable stays 0 the second process will have to wait.

iii.
The loser variable is saving the process ID that was last called to "enter_region()". 
It is taking care that there is only one process in the critical region at the same time. Afte that the variable is changed to the second process.

iv.
```
int loser;
boolean interested = new boolean[3];

void enter_region(int process){
	int other, other2;
	switch(process){
		case 0:
			other = 1;
			other2 = 2;
			break;
		case 1:
			other = 0;
			other2 = 2;
			break;
		case 2:
			other = 0;
			other2 = 1;
			break;
}
	interested[process] = true;
	loser = process;
	while(loser == process && (interested[other] || interested[other2]);
}

void leave_region(int process){
	interested[process] = false;
}
```