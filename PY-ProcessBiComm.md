```
# process_a.py

import subprocess
import threading


# Function to send data to process B
def send_data(proc, data):
    print(f"Sending to B: {data}")
    proc.stdin.write(data)  # Ensure data ends with a newline character
    proc.stdin.flush()  # Ensure data is sent
    proc.stdin.close()  # Close stdin to signal EOF to the child process


# Function to receive data from process B
def receive_data(proc):
    data_from_b = proc.stdout.readline()  # Read the response line
    print(f"Received from B: {data_from_b.strip()}")  # Strip the newline for printing


# Initialize Process A (parent)
proc = subprocess.Popen(['python', 'process_b.py'],
                        stdin=subprocess.PIPE,
                        stdout=subprocess.PIPE,
                        stderr=subprocess.PIPE,
                        text=True,
                        bufsize=1)  # Use line buffering

data_to_send = "Hello from A!"

# Create threads for sending and receiving data
send_thread = threading.Thread(target=send_data, args=(proc, data_to_send))
receive_thread = threading.Thread(target=receive_data, args=(proc,))

send_thread.start()
receive_thread.start()

# Wait for threads to complete
send_thread.join()
receive_thread.join()

```

```
# process_b.py

import sys

# Read data from A line by line
data_from_a = sys.stdin.readline().strip()  # Read a single line and remove the newline character
# print(f"Received from A: {data_from_a}", flush=True)

# Send data to A
data_to_send = "Hi from B!"
print(data_to_send, flush=True)  # Using print automatically adds a newline

```
