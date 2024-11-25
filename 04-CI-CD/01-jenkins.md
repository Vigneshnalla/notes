- It is impossible to install everything one master node like one tema requires java tema version 7 , one team 8 there will be depency conflicts
so resolve this we use nodes 



for suppose we have 3 nodes one is repsonsible for linux env , one for macc and one for windows

the problem is the requriemtn for change in windows is  very rarely 
so resource is getting waste
it is always sitting idle

Using Jenkins with docker as agents

what we will try to do is rn this jenkins pipeline on docker containers so everybody

efficienty resource usage ,

docker daemon proces that's a single process or single source of truth
this daemon process by default is not acessible to other users 
to give acess we need to assing dockergrup to that suer
grants aces to jenkisn user

install docker plugin doker pipeline

configuring docekr as agent:
agent {
   docker {image; node js alpine
   }
}



FreeStyle project and its disadv
