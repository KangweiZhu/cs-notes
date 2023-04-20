```
1. no packet loss, no ACK loss


2. packet loss everyn with N = 3, no ACK loss


3. packet loss everyn with N = 10, no ACK loss


4. no packet loss, ACK loss everyn with N = 3


5. no packet loss, ACK loss everyn with N = 10


6. packet loss alteveryn with N = 8, no ACK loss


7. no packet loss, ACK loss alteveryn with N = 8


8. packet loss iid with N = 5, no ACK loss


9. no packet loss, ACK loss iid with N = 5


10. packet loss everyn with N = 3, ACK loss alteveryn with N = 4


11. start the sender first, give a few seconds, then start the receiver (with any of the loss behaviors
mentioned above)

```



1. no packet loss, no ACK loss

   ```bash
   python receiver.py --pktloss noloss	--ooo_enabled			
   ```

2. packet loss everyn with N = 3, no ACK loss

   ```bash
   python receiver.py --pktloss everyn --pktlossN 3 --ooo_enabled   
   ```

3. pakcet loss everyn with N = 10, no ACK loss

   ```bash
   python receiver.py --pktloss everyn --pktlossN 10 --ooo_enabled	
   ```

4. no packet loss, ACK loss everyn with N = 3

   ```bash
   python receiver.py --pktloss noloss --ackloss everyn --acklossN 3 --ooo_enabled	
   ```

5. no packet loss, ACK loss everyn with N = 10

   ```
   python receiver.py --pktloss noloss --ackloss everyn --acklossN 10 --ooo_enabled
   ```

6. packet loss alteveryn with N = 8, no ACK loss

   ```
   python receiver.py --pktloss alteveryn --pktlossN 8 --ooo_enabled
   ```

7. no packet loss, ACK loss alteveryn with N = 8

   ```
   python receiver.py --pktloss noloss --ackloss alteveryn --acklossN 8 --ooo_enabled
   ```

8. packet loss iid with N = 5, no ACK loss

   ```
   python receiver.py --pktloss iid --pktlossN 5 --ooo_enabled
   ```

9. no packet loss, ACK loss iid with N = 5

   ```
   python receiver.py --ackloss iid --acklossN 5 --ooo_enabled
   ```

10. packet loss everyn with N = 3, ACK loss alteveryn with N = 4

   ```
   python receiver.py --pktloss everyn --pktlossN 3 --askloss alteveryn --acklossN 4 --ooo_enabled
   ```

   





```
	inputs = [cs]
    outputs = []
    last_acked = INIT_SEQNO
    final_ack = INIT_SEQNO + content_len
    first_to_tx = transmit_entire_window_from(win_left_edge) ##transmit_entire_window_from(win_left_edge)
    while win_left_edge < INIT_SEQNO + content_len:
        readable, _, _ = select.select([cs], [], [], RTO)

        if readable:
            (data, sender) = cs.recvfrom(100)
            msg = Msg.deserialize(data)
            ##print("[S]: Received    {}".format(str(msg)))
            ##print("msg ack", msg.ack)
            if msg.ack > last_acked:  # update last ack to highest ack num
                last_acked = msg.ack

                # update window
                win_left_edge = last_acked
                win_right_edge = min(win_left_edge + win_size,
                                     INIT_SEQNO + content_len)

                # check if there is more data to transmit
                if win_right_edge <= final_ack:
                    first_to_tx = transmit_entire_window_from(first_to_tx)

            if last_acked == final_ack:
                break  # all required acks received
        else:  # timeout
            transmit_one()
```





curbest

```
	resend = False
    last_acked = 0
    first_to_tx = 0
    final_ack = INIT_SEQNO + content_len
    while True:
        # time.sleep(1)
        # print("final ack: %s" % final_ack)
            temp = win_left_edge
            win_left_edge = transmit_entire_window_from(win_left_edge)
            win_right_edge = min(win_left_edge + win_size,
                                 INIT_SEQNO + content_len)
            first_to_tx = win_left_edge
            # print("first to transmit {0}".format(first_to_tx))
        while (last_acked < first_to_tx):
            input = [cs]
            inputready, outputready, exceptready = select.select(
                input, [], [], RTO)
            # if exceptready != []:
            #     # print("Error")
            if inputready != []:
                s = inputready[0]
                data_from_receiver, rec_addr = cs.recvfrom(100)
                ack_msg = Msg.deserialize(data_from_receiver)
                
                
                # print("Received: {0}".format(str(ack_msg)))
                acknum = ack_msg.ack
                last_acked = acknum
                
                
            else:
                # print("TIMED OUT")
                win_left_edge = temp
                win_left_edge = transmit_one()
                first_to_tx = win_left_edge
            if (last_acked == final_ack):
                break
        if (last_acked == final_ack):
            break
```



need improve

```
	inputs = [cs]
    while win_left_edge < INIT_SEQNO + content_len:
        
        last_acked = INIT_SEQNO #tracks the highest cumulative ACK number
        final_ack = INIT_SEQNO + content_len

        while True:
            
            readable, writable, errors = select.select(inputs, [], [], RTO)

            if (readable):
                
                data_from_receiver, receiver_addr = readable[0].recvfrom(100)
                ack_msg = Msg.deserialize(data_from_receiver)

                if ack_msg.ack > last_acked:
                    first_to_tx = transmit_entire_window_from(win_left_edge)
                    last_acked = ack_msg.ack
                    win_left_edge = ack_msg.ack 
                    win_right_edge = min(win_left_edge + win_size, INIT_SEQNO + content_len)
                    transmit_entire_window_from(win_left_edge)
                    
                if(last_acked == (INIT_SEQNO + content_len)):
                    break
            else:
                first_to_tx = transmit_one()
```







best

```
tx = transmit_entire_window_from(win_left_edge)
    final_ack = INIT_SEQNO + content_len
    last_ack = 0
    while True:
        inputs = [cs]
        readable, writeable, error = select.select(inputs, [], [], RTO)
        if readable:
            data_from_reciever, reciever_addr = cs.recvfrom(100)
            ack = Msg.deserialize(data_from_reciever)
            if ack.ack > last_ack:
                last_ack = ack.ack
                win_left_edge = last_ack
                win_right_edge = min(win_left_edge + win_size,
                                     INIT_SEQNO + content_len)
                tx = transmit_entire_window_from(tx)
            if last_ack == final_ack:
                break
        if not readable:
            tx = transmit_one()
```





```
inputs = [cs]
    last_acked = 0 # initialize and maintain a variable to track the highest cumulative ack.
    while win_left_edge < INIT_SEQNO + content_len:
        last_acked = INIT_SEQNO
        final_ack = INIT_SEQNO + content_len
        while True:
            readable, writable, errors = select.select(inputs, [], [], RTO)
            if readable:
                data_from_receiver, receiver_addr = readable[0].recvfrom(100)
                message = Msg.deserialize(data_from_receiver)
                if message.ack > last_acked:
                    first_to_tx = transmit_entire_window_from(win_left_edge)
                    win_left_edge = message.ack
                    win_right_edge = min(win_left_edge + win_size, INIT_SEQNO + content_len)
                    last_acked = message.ack
                    transmit_entire_window_from(win_left_edge)      

                    # 4.4 more data to transmit?
                    if win_right_edge <= final_ack:
                        first_to_tx = transmit_entire_window_from(first_to_tx)
                # 4.5 if equal, then stop the tranmission
                if last_acked == final_ack:
                    break
            # 4.6 handle timeou
            else:
                first_to_tx = transmit_one()
```





v1.1

```
inputs = [cs]
    last_acked = 0 # initialize and maintain a variable to track the highest cumulative ack.
    while win_left_edge < INIT_SEQNO + content_len:
        last_acked = INIT_SEQNO
        final_ack = INIT_SEQNO + content_len
        while True:
            readable, writable, errors = select.select(inputs, [], [], RTO)
            if readable:
                data_from_receiver, receiver_addr = readable[0].recvfrom(100)
                message = Msg.deserialize(data_from_receiver)
                if message.ack > last_acked:
                    first_to_tx = transmit_entire_window_from(win_left_edge)
                    win_left_edge = message.ack
                    win_right_edge = min(win_left_edge + win_size, INIT_SEQNO + content_len)
                    last_acked = message.ack
                    transmit_entire_window_from(win_left_edge)
                    # 4.4 more data to transmit?
                    if win_right_edge <= final_ack:
                        first_to_tx = transmit_entire_window_from(first_to_tx)
                # 4.5 if equal, then stop the tranmission
                if last_acked == final_ack:
                    break
            # 4.6 handle timeou
            else:
                first_to_tx = transmit_one()
```





v1.3

```
inputs = [cs]
    last_acked = 0 # initialize and maintain a variable to track the highest cumulative ack.
    while win_left_edge < INIT_SEQNO + content_len:
        # 4.2 initial and maintaina variable last_acked
        last_acked = INIT_SEQNO
        final_ack = INIT_SEQNO + content_len
        while True:
            readable, writable, errors = select.select(inputs, [], [], RTO)
            if readable:
                data_from_receiver, receiver_addr = readable[0].recvfrom(100)
                message = Msg.deserialize(data_from_receiver)
                if message.ack > last_acked:
                    first_to_tx = transmit_entire_window_from(win_left_edge)
                    win_left_edge = message.ack
                    win_right_edge = min(win_left_edge + win_size, INIT_SEQNO + content_len)
                    last_acked = message.ack
                    # send a burst of packets starting from the current left edge of the winodw
                    transmit_entire_window_from(win_left_edge)
                    # 4.4 more data to transmit?
                    if win_right_edge <= final_ack:
                        first_to_tx = transmit_entire_window_from(first_to_tx)
                # 4.5 if equal, then stop the tranmission
                if last_acked == final_ack:
                    break
            # 4.6 handle timeout
            else:
                first_to_tx = transmit_one()
```





```
last_acked = INIT_SEQNO
    final_ack = INIT_SEQNO + content_len
    while win_left_edge < INIT_SEQNO + content_len:
        inputs = [cs]
        readable, writable, error = select.select(inputs, [], [], RTO)
        if readable:
            data_from_receiver, receiver_addr = cs.recvfrom(100)
            message = Msg.deserialize(data_from_receiver)
            acked = message.ack
            if acked > last_acked:
                last_acked = acked
                win_left_edge = last_acked
                win_right_edge = min(win_left_edge + win_size, INIT_SEQNO + content_len)
                first_to_tx = transmit_entire_window_from(win_left_edge)
                if last_acked == final_ack:
                    break
                # 4.4  more data?
                if win_right_edge <= final_ack:
                    transmit_entire_window_from(first_to_tx)
        else:
            # 4.5 handling timeout
            first_to_tx = transmit_one()
            #win_left_edge = first_to_tx
```

