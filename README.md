# aimapi
Invensys (Foxboro) aimapi server and client application. The purpose of this application is to query the invensys AIM * Historian for the historical and real time data.

The application is written in golang-1.4.2 and consists of the following parts:

## Server
Implements the RPC (Remote Procedure Call) server exposing functionality that can be called by a client over the network. The server runs on windows Aim*Historian machine.

* nrg-aimapi-server.exe

This is the API server with a listener on  the port 51295. The listener listens for the incoming requests and passes them to the server. The server invokes the requested functionality and returns the response back to the client. 
Server uses the aimhistory.dll supplied by Invensys and NRG_aimapi_wrapper.dll for invoking various AIM API functions. The client & server use gob encoding for data serialization over the network.The server can be runs as a scheduled task and can be configured to run upon reboot. 

* NRG_aimapi_wrapper.dll:

This is a wrapper dll for aimhistory.dll  and is written in c. The wrapper dll is needed because standard Windows ABI( win32 api) does not  accept more than 16 arguments in any of its exposed API functions but in case of Invensys AIM API there are functions which accept more than 16 arguments. Wrapper DLL is written  in C exposes two functions NRG_an_hist_values &  NRG_fh_RTPDef which in turn call the  an_hist_values and fh_RTPDef aim API functions respectively. This wrapper dll is used by the server program above to expose its functionality over the network using rpc and it has to be in the same directory as the server executable.


## Client
Implements the RPC client that invokes the procedures exposed by server program running on a remote machine and displays the result of the called procedure.
*	Aimapi-client.sparc 

For running on a sparc machine
*	Aimapi-client.linux

For running on a linux machine
* Aimapi-client.exe
 
For windows

The client is also written in golang-1.4.2 . The client uses a configuration file named client.ini which must be in the same directory as the aimapi-client executable. The format of the client.ini file is given below:

<pre>#Format of the file is: historian = ipaddress:port. one historian per line
hist1 =  192.168.1.1:51295
hist2 =  192.168.1.2:51295
hiht3 =  192.168.1.3:51295
hist4 =  192.168.1.4:51295
hist5 =  192.168.1.5:51295
hist6 =  192.168.1.6:51295
</pre>
