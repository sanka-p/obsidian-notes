```bash
virtualenv venv
source venv/bin/activate
python -m pip install --upgrade pip
python -m pip install grpcio
python -m pip install grpcio-tools

python -m grpc_tools.protoc -I../../protos --python_out=. --pyi_out=. --grpc_python_out=path/to/protofile
```

```proto
syntax = "proto3";

service StateMachineService {
  rpc ChangeState(ChangeStateRequest) returns (StateResponse);
}

message ChangeStateRequest {
  string event = 1;
}

message StateResponse {
  string currentState = 1;
}

```

```python
#GRPC Client to manipulate the state of the server
import grpc
import stateMachine_pb2
import stateMachine_pb2_grpc
import time


def change_state(stub, event):
    response = stub.ChangeState(stateMachine_pb2.ChangeStateRequest(event=event))
    print("Current state: " + response.currentState)
    return response.currentState


def run():
    with grpc.insecure_channel('localhost:50051') as channel:
        stub = stateMachine_pb2_grpc.StateMachineServiceStub(channel)
        
        print("--------------")
        print("Changing state to running")
        print("--------------")
        print(change_state(stub, "run"))
        print("--------------")
        print("Changing state to stopping")
        print("--------------")
        print(change_state(stub, "stop"))
        print("--------------")
        print("Changing state to running")
        print("--------------")
        print(change_state(stub, "stopped"))
        
run()
    

```

```python
import stateMachine_pb2
import stateMachine_pb2_grpc
import grpc
from concurrent import futures

class StateMachine:
    def __init__(self,transitions,initial_state):
        self.transitions = transitions
        self.current_state = initial_state 
        
    def change_state(self,action):
        if action in self.transitions[self.current_state]:
            self.current_state = self.transitions[self.current_state][action]
            return True        
        else:
            #print("Invalid action")
            return False
    def get_current_state(self):
        return self.current_state
    
transitions = {"start":{"run":"running"},
               "running":{"stop":"stopping"},
               "stopping":{"stop":"stopping","stopped":"start"}
               }
# Assuming StateMachine class is already defined
class StateMachineServiceServicer(stateMachine_pb2_grpc.StateMachineServiceServicer):
    def __init__(self):
        self.state_machine = StateMachine(transitions, initial_state='start')

    def ChangeState(self, request, context):
        success = self.state_machine.change_state(request.event)
        if success:
            return stateMachine_pb2.StateResponse(currentState=self.state_machine.current_state)
        else:
            # Handle invalid transitions
            context.set_code(grpc.StatusCode.INVALID_ARGUMENT)
            context.set_details('Invalid transition')
            return stateMachine_pb2.StateResponse()
    
    def GetCurrentState(self):
        return stateMachine_pb2.StateResponse(currentState=self.state_machine.current_state)
    

def serve():
    server = grpc.server(futures.ThreadPoolExecutor(max_workers=10))
    stateMachine_pb2_grpc.add_StateMachineServiceServicer_to_server(StateMachineServiceServicer(), server)
    server.add_insecure_port('[::]:50051')
    server.start()
    server.wait_for_termination()

if __name__ == '__main__':
    serve()


```