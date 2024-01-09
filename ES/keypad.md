# Keypad

## connection
3x3:

row0---pb0---pb1---pb2
        |     |     |
row1---pb0---pb1---pb2
        |     |     |
row2---pb0---pb1---pb2
        |     |     |    
       col0  col1 col2

- used 6 mc pins to connect 9 switches
- configure row input, col output or vice versa

## example for default configuration
- rows: input pullup (always high)
- columns: output high 

## functions
- keypad_inti()
pin connections & handle rows to be i/p pu & cols to be o/p high
- keypad_getKey(*ptr) { (signaling)
    ptr = pressed key
    or
    ptr = default value (not pressed)

    for each column {
        col = output low
        for each row {
            read input
            if input == zero
                key pressed
            else if input == high
                key not pressed
            once a switch is detected -> break;
            key id [row] [col]
        }
        col = output high again
    }
}

## configurations 
- make sure to support different keypad sizes
    - n col
    - n row
- connection
    - port
    - pin
- optional configuration: each key character
    - return key number
    - return character (better) 
    [row][col] = {
        {1,2,3,A},
        {4,5,6,B},
        ...
    }

## switch debouncing
in switch driver
by timers, RTOS, scheduler
switch_states[] = {
    start = pressed
    stop = not pressed
    inc
    dec
}
switch task every 5ms checks which switch pressed (reads all)
how to avoid bouncing?
create counter for each button
if for 5 tasks switch reading is same?
--> switch state is pressed
reading repeated 5 or 10 times --> actual reading

- get_switch_state(switch,ptr_state) {
    doesnt read HW
    gets value from array that's updated by task every 5 ms

    ptr_state = switch_state[switch]
}

another way = state machine

## 

