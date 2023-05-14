**Conjugation Microservice Usage Guide-**

**REQUESTING data from microservice:**
To request data from the microservice, a message should be sent in list format, with the index 0 being a string holding
the verb currently being conjugated. Index 1 will hold a digit in range 0-6 with each value representing a pronoun "group".
In order, these groups are 'yo', 'tú', ('el', 'ella', 'usted'), 'nosotros', 'vosotros', and ('ellos', 'ellas', 'ustedes'). 
Index 2 will then contain the user's input for what they believe the conjugation is for that verb and pronoun/pronoun group.

From here, the app will employ a zmq REQ socket, and connect said socket to whichever port is chosen for the microservice.
The app will prepare the above message list, and use the send_json instruction to send the request along with the message.
See below for a simple example:

import zmq

context = zmq.Context()
socket = context.socket(zmq.REQ)   #creating REQ socket

socket.connect("tcp://localhost:7777") #connecting socket to port 7777

message = ["tener", 1, "tienas"]  #formatted message in list

socket.send_json(message) #message being converted and sent as JSON

**RECEIVING data from microservice:**
Receiving data from the microservice is very easy, in fact given the nature of the microservice and what is being sent 
back, it can be done in one line. As the microservice is sending back a string, all that has to be done to receive the
data is to use socket.recv(), and set this as the value of a variable. For instance:

answer = socket.recv()

After this, the data can be used as needed until the next call to the microservice. 
See below for a UML diagram displaying how the process works. 

![Assignment8UMLDiagram](https://user-images.githubusercontent.com/102439396/236686571-c244d66c-c4b0-4549-9987-58064a92b4af.png)
