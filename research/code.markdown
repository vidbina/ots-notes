# Snippets
Why the assignment nested within two similar conditional statements?
```java
SelectionProperty sp = (SelectionProperty) ap;
if ("Car following model".equals(sp.getShortName()))
{
    if ("Car following model".equals(sp.getShortName()))
    {
        carFollowingModelName = sp.getValue();
    }
}
```

# Understanding
This document serves to help in understanding how simulations work in OTS

 - `constructModel` is the method within a `XModel` class that defines the simulation model
   - `simulator = (OTSDEVSimulatorInterface) sim`
   - iterate over `properties`
     - assign local properties
   - for every lane
     - create lane
     - add lane to `this.path`

$p_n = p_{n-1} + (X\cdot 2 - 1)\cdot \omega$
$X \sim U([0, 1])$
```java
double actualHeadway = headway + (random.nextDouble() * 2 - 1) * variability;
generateCar(this.lane1, new DoubleScalar.Rel<LengthUnit>(pos, LengthUnit.METER));
pos += actualHeadway;
```

## GTU Creation

```java
new LaneBasedIndividualCar<>(
  ++this.carsCreated, // final ID
  null /* gtuType */,  // final GTUType<?>
  generateTruck ? this.carFollowingModelTrucks : this.carFollowingModelCars,  // final GTUFollowingModel
  this.laneChangeModel, // final LaneChangeModel
  initialPositions, // final Map<Lane, DoubleScalar.Rel<LengthUnit>>
  initialSpeed, // final doubleScalar.Abs<SpeedUnit>
  vehicleLength, // final doubleScalar.Rel<LenghtUnit>
  new DoubleScalar.Rel<LengthUnit>(1.8, LengthUnit.METER),  // final DoubleScalar.Abs<SpeedUnit>
  new DoubleScalar.Abs<SpeedUnit>(200, SpeedUnit.KM_PER_HOUR), // final OTSDEVSSimulatorInterface
  this.simulator,
  DefaultCarAnimation.class
);
```

The `LaneBasedIndividualCar` extends `AbstractLaneBasedIndividualGTU`, 
which extends `AbstractLaneBasedGTU` which extends `AbstractGTU`. The 
constructor for the `AbstractLaneBasedGTU` calls the constructor for 
`AbstractGTU` and execute the following:

 - assign models (following model and lane change model) and lateral velocity
 - for every lane
   - add lane to `this.lanes`
   - add fractional link positions for lane
```java

```

### How does lane registration work?
`lanes` represents an array of all lanes the GTU is registered on

`position`

Within the `position` method of `AbstractLaneBasedGTU` the following is 
executed upon the creation of a GTU.

 - $p_{longitudinal} := l_{lane}\cdot \phi$ where $\phi$ represents the fractional link position
 - $\Delta t = t_\omega - t_{n-1}$ where $\omega$ represents the time for which we want to perform this operation
 - $((p_{longitudinal} + (v_0\cdot \Delta t)) + ((\frac{v_\omega}{t_\omega^2})\cdot \frac{\Delta t^2}{2})) + p_{relative}$

```java
DoubleScalar.Rel<LengthUnit> loc = DoubleScalar.plus(
  DoubleScalar.plus(
    DoubleScalar.plus(
      longitudinalPosition, 
      Calc.speedTimesTime(this.speed, dT)
    ).immutable(), 
    Calc.accelerationTimesTimeSquaredDiv2(this.getAcceleration(when), dT)
  ).immutable(), 
  relativePosition.getDx()
).immutable();
```

`AbstractLaneBasedGTU` `move`

### How do Fractional Link Positions work?
Fractional link positions provide some indication on where a GTU is located on
a given link by manner of value relative to the total length of the link. This 
allows one to specify that a GTU is positioned in the middle of a link without 
prior knowledge of the length of the link.

The counterpart to fractional link positions are longitudinal positions which
give an absolute value for the position of the GTU starting from the origin of
the link. 

