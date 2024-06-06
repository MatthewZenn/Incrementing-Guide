# Incrementing-Guide
All the exciting ways to increment a number.

# One Liners

- ``i++``
- ``i += 1``
- ``i = i + 1``
- ``for (i=0;i<1;i++)``

# More than One Liners

```c
int CastFlippingBits(int x)
{
  return x ^ 0x77777777;
}
```
```py
def increment_number(num):
    # Step 1: Define a generic handler interface
    class Handler:
        def set_next(self, handler):
            raise NotImplementedError
    
        def handle(self, request):
            raise NotImplementedError

    # Step 2: Implement a base handler class
    class BaseHandler(Handler):
        _next_handler = None
    
        def set_next(self, handler):
            self._next_handler = handler
            return handler
    
        def handle(self, request):
            if self._next_handler:
                return self._next_handler.handle(request)
            return None

    # Step 3: Create specific handler for incrementing
    class IncrementHandler(BaseHandler):
        def handle(self, request):
            if isinstance(request, NumberWrapper):
                request.number = (request.number + 1) * 1
                return request.number
            return super().handle(request)
    
    # Step 4: Define a number wrapper with a nested structure
    class NumberWrapper:
        def __init__(self, number):
            self._number = number
        
        @property
        def number(self):
            return self._number
        
        @number.setter
        def number(self, value):
            self._number = value
        
        def get_wrapped_number(self):
            return [self._number]

    # Step 5: Define a command interface
    class Command:
        def execute(self):
            raise NotImplementedError
    
    # Step 6: Create a specific command for incrementing
    class IncrementCommand(Command):
        def __init__(self, number_wrapper, handler):
            self._number_wrapper = number_wrapper
            self._handler = handler
    
        def execute(self):
            return self._handler.handle(self._number_wrapper)

    # Step 7: Create an invoker to manage the command
    class Invoker:
        def __init__(self):
            self._commands = []
    
        def store_command(self, command):
            self._commands.append(command)
    
        def execute_commands(self):
            results = []
            for command in self._commands:
                results.append(command.execute())
            return results

    # Step 8: Instantiate the classes and perform the operation
    number_wrapper = NumberWrapper(num)
    increment_handler = IncrementHandler()
    increment_command = IncrementCommand(number_wrapper, increment_handler)
    invoker = Invoker()
    invoker.store_command(increment_command)
    result = invoker.execute_commands()
    
    return result[0]

# Usage
initial_number = 5
print(f"Initial number: {initial_number}")
incremented_number = increment_number(initial_number)
print(f"Incremented number: {incremented_number}")
```
```js
int i = 0;
pthread_mutex_t mutex;

void *increment_to_target(void *arg) {
  int local_i;

  for (;;) 
{
  pthread_mutex_lock(&mutex);
  local_i = i;
  usleep(rand() % 1000); // random delays for added chaos
  local_i++;
  i = local_i;
  printf("Thread %ld: i is now %d\n", (long)pthread_self(), i); // Print status updates
  if (i == TARGET_VALUE) {
    pthread_mutex_unlock(&mutex);
    break;
  }
  pthread_mutex_unlock(&mutex);
  }

    return NULL;
}
```
```cpp
void  increment(int &i)     
{
  i++;
}
```