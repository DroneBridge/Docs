---
description: Examples on how to use DroneBridge library to send & receive custom data
---

# DroneBridge lib example usage

## DroneBridge Common library

All DroneBridge modules use the DroneBridge Common library to send & receive data. Below are some examples given on how to use the library. It is available for Python and C/C++. Both implementations are native and not directly linked.

{% hint style="info" %}
AES encryption 128, 192 or 256-bit is currently only available for the Python implementation. _Pycryptodomex_ is used.
{% endhint %}

### Python

Link to the DroneBridge class. All examples can also be found inside the git repository.

#### Sending

{% code title="example\_sender.py" %}
```python
#
#   This file is part of DroneBridge: https://github.com/seeul8er/DroneBridge
#
#   Copyright 2019 Wolfgang Christl
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.
#

import time

from DroneBridge import DroneBridge, DBDir, DBMode, DBPort

list_wifi_interfaces = ['wlx00c0ca973410']  # must be in monitor mode
communication_id = 22  # idents a link, multiple systems with same ports on same frequency possible
send_recv_port = DBPort.DB_PORT_GENERIC_1  # Virtual port to which the packet is addressed to
frame_type = 1  # RTS, 2 DATA
compatibility_mode = False  # Enable with unpatched Kernels etc. (try without first)
send_direction = DBDir.DB_TO_UAV  # Direction of the packet. Reverse direction on receiving side needed

# Set to None for unencrypted link. Must be the same on receiving side.
# length of 32, 48 or 64 characters representing 128bit, 192bit and 256bit AES encryption
encrypt_key = "3373367639792442264528482B4D6251"  # bytes or string representing HEX

payload_data = b'HelloEveryone.IamPayload!LifeIsEasyWhenYouArePayload'
dronebridge = DroneBridge(send_direction, list_wifi_interfaces, DBMode.MONITOR, communication_id, send_recv_port,
                          tag="Test_Sender", db_blocking_socket=True, frame_type=frame_type,
                          compatibility_mode=compatibility_mode, encryption_key=encrypt_key)

list_sockets_raw = dronebridge.list_lr_sockets  # List of raw sockets to be used for manual send/receive operations

for i in range(10000):
    time.sleep(0.5)
    dronebridge.sendto_uav(payload_data, send_recv_port)
    print("\r" + str(i), end='')

print("\nDone sending!")
```
{% endcode %}

#### Receiving

{% code title="example\_receiver.py" %}
```python
#
#   This file is part of DroneBridge: https://github.com/seeul8er/DroneBridge
#
#   Copyright 2019 Wolfgang Christl
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.
#

from DroneBridge import DroneBridge, DBDir, DBMode, DBPort

list_wifi_interfaces = ['wlx8416f916382c']  # must be in monitor mode
communication_id = 22
send_recv_port = DBPort.DB_PORT_GENERIC_1
frame_type = 1  # RTS, 2 DATA
compatibility_mode = False
send_direction = DBDir.DB_TO_GND

# Set to None for unencrypted link. Must be the same on receiving side.
# length of 32, 48 or 64 characters representing 128bit, 192bit and 256bit AES encryption
encrypt_key = "3373367639792442264528482B4D6251"  # bytes or string representing HEX

dronebridge = DroneBridge(send_direction, list_wifi_interfaces, DBMode.MONITOR, communication_id, send_recv_port,
                          tag="Test_Receiver", db_blocking_socket=True, frame_type=frame_type,
                          compatibility_mode=compatibility_mode, encryption_key=encrypt_key)

dronebridge.clear_socket_buffers()

for i in range(100):
    received_payload = dronebridge.receive_data()
    if received_payload:
        print("Received: " + received_payload.decode())

```
{% endcode %}

### C/C++

The common library must be linked during compilation. An example `CMakeLists.txt` is given blow. Place the file inside a folder of the root directory of the DroneBridge git project and it should compile right away.

{% code title="CMakeLists.txt" %}
```c
cmake_minimum_required(VERSION 3.5)
project(example)

set(CMAKE_C_STANDARD 11)

add_subdirectory(../common db_common)
set(SRC_FILES_RECV receive_main.c)
set(SRC_FILES_SEND send_main.c)

add_executable(send ${SRC_FILES_SEND})
target_link_libraries(send db_common)

add_executable(receive ${SRC_FILES_RECV})
target_link_libraries(receive db_common)
```
{% endcode %}

#### Sending

{% code title="send\_main.c" %}
```c
/*
 *   This file is part of DroneBridge: https://github.com/seeul8er/DroneBridge
 *
 *   Copyright 2019 Wolfgang Christl
 *
 *   Licensed under the Apache License, Version 2.0 (the "License");
 *   you may not use this file except in compliance with the License.
 *   You may obtain a copy of the License at
 *
 *   http://www.apache.org/licenses/LICENSE-2.0
 *
 *   Unless required by applicable law or agreed to in writing, software
 *   distributed under the License is distributed on an "AS IS" BASIS,
 *   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *   See the License for the specific language governing permissions and
 *   limitations under the License.
 *
 */

#include <signal.h>
#include <stdint.h>
#include <string.h>
#include <stdio.h>
#include <zconf.h>
#include "../common/db_raw_send_receive.h"

// DroneBridge options
#define INTERFACE   "wlx8416f916382c"
#define FRAME_TYPE  2   // 1=RTS; 2=DATA
#define DATARATE    11  // Mbit
#define COMMID      16
#define DST_PORT    10  // destination port
#define RECV_PORT   DST_PORT

#define PAYLOAD_BUFF_SIZ 1024

int keep_running = 1;

void int_handler(int dummy) {
    keep_running = 0;
}

int main(int argc, char *argv[]) {
    signal(SIGINT, int_handler);
    signal(SIGTERM, int_handler);
    char interface[IFNAMSIZ];
    strcpy(interface, INTERFACE);

    db_socket_t raw_socket = open_db_socket(interface, COMMID, 'm', DATARATE, DB_DIREC_GROUND,
                                            RECV_PORT, (uint8_t) FRAME_TYPE);

    uint8_t seq_num = 0;
    uint8_t payload[PAYLOAD_BUFF_SIZ];
    memset(payload, 1, PAYLOAD_BUFF_SIZ);

    while(keep_running) {
        db_send_div(&raw_socket, payload, DST_PORT, PAYLOAD_BUFF_SIZ, update_seq_num(&seq_num), 0);
        printf(".");
        fflush(stdout);
        usleep((unsigned int) 1e6);
    }
    close(raw_socket.db_socket);
    printf("Terminated");
}
```
{% endcode %}

#### Receiving

{% code title="receive\_main.c" %}
```c
/*
 *   This file is part of DroneBridge: https://github.com/seeul8er/DroneBridge
 *
 *   Copyright 2019 Wolfgang Christl
 *
 *   Licensed under the Apache License, Version 2.0 (the "License");
 *   you may not use this file except in compliance with the License.
 *   You may obtain a copy of the License at
 *
 *   http://www.apache.org/licenses/LICENSE-2.0
 *
 *   Unless required by applicable law or agreed to in writing, software
 *   distributed under the License is distributed on an "AS IS" BASIS,
 *   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *   See the License for the specific language governing permissions and
 *   limitations under the License.
 *
 */

/**
 * Example on how to use DroneBridge common library to receive data.
 * This example receives on the port of the status module.
 * Set port & adapter name to match your configuration 
 */

#include <string.h>
#include <stdio.h>
#include <signal.h>
#include <zconf.h>
#include "../common/db_raw_send_receive.h"
#include "../common/db_raw_receive.h"

#define BUFFER_SIZE 2048

#define ADAPTER_NAME "wlx8416f916382c"    // Name of WiFi adapter. Must be in monitor mode
#define DB_RECEIVING_PORT DB_PORT_STATUS  // Receive data on DB raw port for status module
#define DB_SEND_DIRECTION DB_DIREC_DRONE  // Send data in direction of UAV

int keep_going = 1;

void sig_handler(int sig) {
    printf("DroneBridge example receiver: Terminating...\n");
    keep_going = 0;
}

int main(int argc, char *argv[]) {
    uint8_t buffer[BUFFER_SIZE];
    uint8_t payload_buff[DATA_UNI_LENGTH];
    uint8_t seq_num = 0;
    memset(buffer, 0, BUFFER_SIZE);

    uint8_t comm_id = 200;
    char db_mode = 'm';
    char adapters[DB_MAX_ADAPTERS][IFNAMSIZ];
    strncpy(adapters[0], ADAPTER_NAME, IFNAMSIZ);

    // init DroneBridge sockets for raw protocol
    db_socket_t raw_interfaces[DB_MAX_ADAPTERS];  // array of DroneBridge sockets
    memset(raw_interfaces, 0, DB_MAX_ADAPTERS);
    raw_interfaces[0] = open_db_socket(adapters[0], comm_id, db_mode, 6, DB_SEND_DIRECTION,
                                       DB_RECEIVING_PORT, DB_FRAMETYPE_DEFAULT);
    // Some stuff for proper termination
    struct sigaction action;
    memset(&action, 0, sizeof(struct sigaction));
    action.sa_handler = sig_handler;
    sigaction(SIGTERM, &action, NULL);
    sigaction(SIGINT, &action, NULL);
    printf("DroneBridge example receiver: Waiting for data\n");
    while (keep_going) {
        uint16_t radiotap_length = 0;
        ssize_t received_bytes = recv(raw_interfaces[0].db_socket, buffer, BUFFER_SIZE, 0);
        uint16_t payload_length = get_db_payload(buffer, received_bytes, payload_buff, &seq_num, &radiotap_length);
        printf("Received raw frame with %zi bytes & %i bytes of payload\n", received_bytes, payload_length);
    }
    for (int i = 0; i < DB_MAX_ADAPTERS; i++)
        close(raw_interfaces[i].db_socket);
    return 0;
}

```
{% endcode %}

