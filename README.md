# 7nm-FinFET-Circuit-Design-and-Characterization

## Characterization of a 7nm FinFET Inverter
Modified SPICE Deck

### Switching Threshold voltage (VTC)
The switching threshold voltage of an inverter is the input voltage at which the output voltage equals the input voltage on the voltage transfer characteristic (VTC) curve. The threshold voltage can shift left or right depending on the sizing of the PMOS and NMOS transistors.

SPICE command
``` meas dc v_th when nfet_out=nfet_in ```

