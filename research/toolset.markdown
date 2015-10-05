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

## Map Rendering
At this very moment libraries like Leaflet offer quite elaborate feature-sets 
to render maps on HTML canvas. Rendering on HTML canvas provides a method of 
generating map visual that are somewhat platform agnostic, which renders this
solution suitable for Chromebooks, mobile devices and desktop computers alike
as all offer some access to HTML5 compliant browsers.

## Graph Manipulation
At the time of writing one of the closest open-source solutions for graph 
manipulation is to be found in the [iD editor][http://ideditor.com], which 
allows the manipulation of OpenStreetMap content.

The iD editor utilizes the D3 rendering library which allows for the rendering
of vector graphics (SVG-based) on HTML5 canvas.

# Inputs
The following section discusses some of the core input components utilized to
describe simulations. The Open Traffic simulation suite offers the entry of 
input through XML data.

In order to validate the XML, DTD's (Document Type Definitions) are available
which specify the valid structure of Open Traffic XML files.

As of yet there are no solutions available for verifying DTD's in the browser
which would require this element to be handled by some auxiliary logic 
implemented otherwise (Java, C/C++ or Python application).

## Actors
The _ots-core_ project as of revision 878 (the revision examined at the time of
writing) facilitates the input of infrastructure through XML representation.

The `GTU` resource provides a means for describing traffic units which, in the
context of the Open Traffic suite` represent vehicles functioning as actors 
within the simulation.

```xml
<NETWORK xmlns="http://www.opentrafficsim.org/ots-infra" 
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.opentrafficsim.org/ots-infra ots-infra.xsd">

  <GLOBAL WIDTH="3.6m" SPEED="100km/h" />
  <GTU NAME="CAR" GTUTYPE="CAR" FOLLOWING="IDM+" 
    LENGTH="UNIF(5,7) m" WIDTH="UNIF(1.7, 2) m" 
    LANECHANGE="EGOISTIC" MAXSPEED="CONST(120) km/h" />
  <GTU NAME="TRUCK" GTUTYPE="TRUCK" FOLLOWING="IDM+" 
    LENGTH="UNIF(16,24) m" WIDTH="UNIF(2.2, 2.7) m" 
    LANECHANGE="ALTRUISTIC" MAXSPEED="CONST(100) km/h" />
</NETWORK>
```

In order to utilize described `GTU`'s in simulations, generators have to be 
specified which control the initial speed of vehicles added unto the 
infrastructure and the distribution between different vehicle types on given 
lanes among other properties. The following snippet provides the description of
a section of infrastructure into which vehicles are injected through three 
generators.

```xml
<LINK NAME="ENTRY" FROM="NE" TO="N1" ELEMENTS="S1-X1|A1:D:A2|X2-S2">
  <STRAIGHT LENGTH="200m" />
  <LANE NAME="A1" SPEED="100 km/h" />
  <LANE NAME="X1" WIDTH="2.5m"/>
  <LANE NAME="X2" WIDTH="2.5m"/>
  <LANE NAME="S1" WIDTH="2m"/>
  <LANE NAME="S2" WIDTH="2m"/>
  <GENERATOR LANE="A1" GTU="CAR" IAT="EXPO(30) s" 
    INITIALSPEED="TRIA(80,90,100) km/h" MAXGTU="50" />
  <GENERATOR LANE="A1" GTU="TRUCK" IAT="EXPO(60) s" 
    INITIALSPEED="TRIA(60,70,80) km/h" MAXGTU="25" />
  <GENERATOR LANE="A2" GTU="CAR" IAT="EXPO(45) s" 
    INITIALSPEED="TRIA(80,90,100) km/h" MAXGTU="25" />
</LINK>
```

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
The goal of this project is to develop a cross-platform solution which 
facilitates the entire simulation work-flow to the end-user. This means that 
this solution should allow the submission of the simulation description to
the simulation engine and present the generated results back to the user, 
subsequently allowing for simulation iterations in which the user may 
manipulate simulation parameters and observe the resulting output.

For the sake of clarity it is important to specify that this project covers the
development of a front-end.

## Integration
There are parts of the application logic that will require communications 
between the simulation engine and the front-end, which is written in Java. As 
of yet, methods are available for submitting simulation details into the 
Java-based engine.

Projects like Electron and TideKit provide means for shipping web-based 
applications to multiple platforms, both desktop and mobile.

<!--
## Graph Representation & Modification
## Simulation Rendering

 - Does the Animator offer hooks to handle customized drawing.
 - Mechanisme om data te exporteren vanuit OTS
-->
