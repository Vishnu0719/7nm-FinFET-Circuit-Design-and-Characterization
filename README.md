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
```
let id = v2#branch
plot id

```
### Power Consumption (P)

Power consumption is the integral of the product of supply voltage (Vdd) and transient current over a time period.

SPICE Command:

``` 
let trans_current = v2#branch
    meas tran id_pwr integ trans_current from=2e-11 to=6e-11
    let pwr = id_pwr * 0.7
    let power = abs(pwr / 40e-12)
    print power
```

### Propagation Delay (Tp)
Propagation delay is the time taken for a signal to propagate through the inverter, measured from 50% of the input rise/fall voltage to 50% of the output fall/rise voltage.

Rise time (Tr): Time taken for the output to rise from 10% to 90% of its maximum value.
Fall time (Tf): Time taken for the output to fall from 90% to 10% of its maximum value.

SPICE Command:
```
    meas tran tpr when nfet_in = 0.35 rise = 1
    meas tran tpf when nfet_out = 0.35 fall = 1
    let tp = (tpr + tpf) / 2
    print tp
```

### Gain (Av)
Voltage gain is defined as the rate of change of output voltage with respect to the input voltage.

SPICE Command:
```
let gain_av = abs(deriv(nfet_out))
plot gain_av

```
### Noise Margin (NM)
The noise margin of an inverter measures its tolerance to input noise while still correctly recognizing logic levels. It is defined using four voltage parameters:

VIL: Maximum input voltage recognized as logic low
VIH: Minimum input voltage recognized as logic high
VOL: Output voltage for logic low
VOH: Output voltage for logic high


Noise Margin High (NMH) is given as, NMH = VOH - VIH

Any input signal within this range is detected as logic HIGH.

Noise Margin Low (NML) is given as, NML = VIL - VOL

Any input signal within this range is detected as logic LOW.

SPICE Command:
```
    meas dc vil find nfet_in when gain_av = gain_target cross=1 
    meas dc voh find nfet_out when gain_av = gain_target cross=1
    meas dc vih find nfet_in when gain_av = gain_target cross=2
    meas dc vol find nfet_out when gain_av = gain_target cross=2
    let nmh = voh - vih
    let nml = vil - vol
```

### Transconductance (gm)
It is the ratio of the change in drain current (Id) to the change in gate to source voltage (Vgs).
SPICE Command:
```
   let gm = real(deriv(id, nfet_in))
    meas dc gm_max MAX gm
    plot gm
```

### Frequency
It is calculated as the inverse of total propagation delay.

F = 1/(tr+tf)

SPICE Command:
```
    tran 0.1 100p                         
    meas tran tr when nfet_in=0.07 RISE=1  
    meas tran tf when nfet_out=0.63 FALL=1 
    let t_delay = tr + tf                  
    print t_delay                         
    let f = 1/t_delay                     
    print f      

```
## 7nm FinFET Bandgap Reference Design and Simulation using Xschem





