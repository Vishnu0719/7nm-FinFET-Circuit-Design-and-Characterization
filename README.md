# 7nm-FinFET-Circuit-Design-and-Characterization

## Characterization of a 7nm FinFET Inverter
Modified SPICE Deck

### Switching Threshold voltage (VTC)
The switching threshold voltage of an inverter is the input voltage at which the output voltage equals the input voltage on the voltage transfer characteristic (VTC) curve. The threshold voltage can shift left or right depending on the sizing of the PMOS and NMOS transistors.

SPICE Command:
``` meas dc v_th when nfet_out=nfet_in ```

### Drain Current (Id)
Drain current (Id) is the current flowing from the source to the drain of a FinFET when a gate voltage is applied and a voltage is present between the source and drain.

In a linear region, Id increases linearly with respect to Vds

In the saturation region (i.e when Vds > Vgs-Vth), Id becomes independent of the applied voltage, and the transistor behaves like a constant current source.

Unlike planar MOSFETs, a FinFET has a 3D structure where the channel is wrapped by the gate on three sides, providing better electrostatic control over the channel.

SPICE Command:
``` let id = v2#branch
plot id
```


