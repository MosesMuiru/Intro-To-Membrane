pipeline
-> Create streaming application with memebrane
    alos to statrt element and data flow btn them
    pipeline(parent) -> element(childre)
linking -> connecting elements together
    use Membrane pipeline behauviour, 
    we implement the membrane callbacks


Elements
    Basic entities responsible for processing
    communicate by message passing  
    started and controlled by parrents
    
    source -> fetches stream from outside pipeline and being it to the element
    sink -> consumes stream from each element
    filter -> receives streams, process and sends to other lements
    endpoint(source and sink) -> can both deliver and consume the data from other elements

    Elements implentation
    consists if pads, options, callbacks

    1. pads
    allow creation of flow of data bethween elments,
    two types, input and output
    output pads, source, filter and endpoint
    inputs pads, filter, sink, endpoint
    Should define the format of data
    
        Defining pads
        use def_input_pad, and def output pads
        if element has a single input and output pad, convention is input, output

        properties
        ----------
        accepted_formats -> this is the pattern expected on the pad
        flow_control -> configures how back pressure should be :wqhandled on the pad
            :auto > 
            :manual >
            :push >
        demand_unit -> 
            :bytes, :buffers 

    2. Options
    allow confi data to an element--> use def_options
    ```sh
        def_options my_options: [
        
            spec: integer() | string(),
            default: 0, 
            description: ""
            ], 
            option_two
        
    ```
    3. Callbacks
    have diff resposibilities and are called in specific order
    
        1. handle_init -> upon lement creation,    recieves options set
        2. handle_setup > for resource allocation or some potential time consuming init,
            this can follow
            2.a handle_pad_added, handle_pad_removed 
            2.b handle_parent_notification> called whnever parent sends a notification
        3. handle_playing > when stream processing is ready
            1. stream format > tells the subsequednt lement what kind of stream it should expect to given pad
            2. buffer > sends media data to seubsquent elemtn
            3. event
            4. demand
            5. end_of_stram >stram has finished

        4. handle_stream_format > tells you what kind of stream you should expct on the given pad(stage)
        5. handle_start_ofstream > before preceding element
        6. handle_buffer > called when a buffer arrives from a preceding element
        7. handle_event 



Bins
    Containers for elements(entities for processing)
    can be nested
    http bin > recieve audio and video streams, payloading and cmaf muxing(process of packaing individual videa streams into a simple containerjk)
