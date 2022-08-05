Known Issues and Limitations
======================

    1. Static dual antennae heading initialization is not yet enabled.
    
    2. System-time jitter has been occasionally observed in the output messages of the ANELLO EVK (in both APIMU and APINS messages). The severity of the jitter increases with unit on-time and is more noticeable in data messages collected over the USB channel than via UDP.
 
       In order to limit the jitter in the output messages, we recommend using UDP as the primary communication channel for data collection whenever possible.
 
       We are currently working on a major firmware update that will include, among other items, improvements in system messaging. Future messages will be available in two formats: ASCII and binary. ASCII will be provided for convenience as it is readable without the need for conversion into human-readable form. Binary versions of the same messages will also be available. This will become the recommended format for output messages, given the compactness of the messages relative to the size of the equivalent ASCII message, and minimized time slippage due to the processing of longer ASCII messages. UDP will remain the recommended communication channel.
