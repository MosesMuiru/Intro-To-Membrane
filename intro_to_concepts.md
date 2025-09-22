Elements types

    source -> fetch streams from outside and delivers to other elments
    sink -> consumes the stream from other elements, process it and sends it down 
    endpoint -> sink, source

pads
creation of flow of data btn elements
input -> how data comes in
output -> how data goes out and event the format

to send data btwn elemeents their pads need to be linked 


rules for linking pads
    one pad of an element can only be linked with one pad of another lement
    only links bwn output and input pads are allowed
    have the same stream formats


defining pads
    if element has a sinlle input and output pad
    convention name it input and output, 
    name and properties
    properties

        1. accepted_format -> stream format for that pad, e.g %RawVideo{}

        2. flow_control -> how back pressure should be handled
            :auto -> membrane does the control
            :manual -> manual control the the flow, use deman action on input pads
                        output pads use handle_demand callback
            :push -> element producting data pushes it through output pad

        3. demand_unit-> :bytes, :buffers - units used to request and recieve demands
        

        4. availability -> 
            :always(default) - available when an element is started
            :on_request - dynamic, 

        5. options(def_options)
            pass configuraton data to an elements 
                ```

                    def_options option_one: [

                            spec: integer(), 
                            description: "should provide a simple description of the otions"
                            ], 
                            option_two[
                            spec: non_integer()
                            ]
                ```


callbacks
interact with the elements by returning actions
is a way of elemetn interaction with other elemen
    
handle_init
upon leemtn creation

handle_setup
resource allocation and initialization that take much resource and time

handle_pad_added -> when new pad is added to an element
handle_pad_removed -> 

handle_parent_notification
invocked when a message from parent is recieved, by defaults it ignores the received message
whenever the parents sends a notification to the element, 

handle_playing
when stream process is ready to start
returns: 
    stream_format-> tells the next element heeey this is what is comming your way
    buffer -> sends media data to the subsequedt element, but stream format has to be sent first
    event -> sends a custom struct to the subsequent or preceding element downstream
    demand -> request data from previous element, flow_control: manual mode
    end_of_streams -> tells the next elemnt, yoooooh, streaming has finished

after handle_playing
handle_stream_format -> tells you the kind of stream you should expect on a given pad
handle_start_of_stream -> is called just before he first buffer arrives
handle_buffer -> called every time a buffer arrives from a preceding element
handle_event -> callled on events arrives from preceding element
handle_demand -> called when element want tot request data(flow_control: manual)


Pads and Linking

dynamic pads
acts as a template
when the number of pads is not known

use :on_request

and handle when the pad is add or when the pad is removed(handle_pad_added and handle_pad_removed)

```

to make a pad dynamic, you need to set its availability to :on_request in def_input_pad or def_output_pad.

each time some element is linked to this pad, a new instance of the pad is created and handle_pad_added callback is invoked. Instances of a dynamic pad can be referenced as Pad.ref(pad_name, pad_id). When a dynamic pad is unlinked, the handle_pad_removed callback is called.
```

Tee
-> allows forwading stream from single input to multiple outptus



