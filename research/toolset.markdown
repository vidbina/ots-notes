# Toolset
In this document some tools are explored which may be used for the development 
of a graphical user interface for the OpenTraffic simulation suite. Within the 
context of the simulation suite, a few requirements are to be kept in 
consideration while evaluating the merrits of different tools. The following 
list enumerates some design goals to be accomplished through the GUI solution.

 - facilitate the modification of a graph utilizing:
    - click or tap actions to create objects at the origin specified by the pointer
    - drag-and-drop functionality to move existing resources (vertices and edges)
 - facilitate the information exchange between the simulation suite and the front-end GUI
 - represent visible components in a simulation, at the appropriate places and in their proper states within 1ms after the computation by the simulator

In order to provide the proper context the manner in which infrastructure and 
actor inputs are represented within the simulation suite are briefly discussed.

The available platforms this solution can be developed for will be discussed to
provide the reader with some indication of the challenges involved with each
platform. Each platform may have different tools at its disposal. For this very
reason, the tools will be discussed per platform.

# Inputs
## Actor
## Infrastructure
The _ots-core_ project as of revision 878 (the revision examined at the time of
writing) facilitates the input of infrastructure through XML representation.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<NETWORK xmlns="http://www.opentrafficsim.org/ots-infra"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.opentrafficsim.org/ots-infra ots-infra.xsd">

  <GLOBAL WIDTH="3.6m" SPEED="80km/h" />

  <NODE NAME="N1" COORDINATE="(0.0, 0.0)" ANGLE="90" />
  <NODE NAME="N2" />
  <NODE NAME="NE" />

  <LINK NAME="L1" FROM="N1" TO="N2" ELEMENTS="A1:D:A2">
    <ARC RADIUS="1000m" ANGLE="180" DIRECTION="L" />
  </LINK>

  <LINK NAME="L2" FROM="N2" TO="N1" ELEMENTS="A1:D:A2">
    <ARC RADIUS="1000m" ANGLE="180" DIRECTION="L" />
  </LINK>

  <LINK NAME="ENTRY" FROM="NE" TO="N1" ELEMENTS="A1:D:A2">
    <STRAIGHT LENGTH="200m" />
  </LINK>

</NETWORK>
```

# Platforms
## Graph Representation & Modification
## Simulation Rendering
