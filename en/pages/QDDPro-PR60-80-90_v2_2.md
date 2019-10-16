# Parameter introduction
## QDD Pro-PR60-80-90 Parameter Diagram[mm]
![QDD Pro-PR60-80-90]( ../img/Qddpro_PR60_x_90_v2_2sanshitu.png )

**Usage and Installation description**

*   Front Fixing and Rear Fixing

![Qddpro_PR60_x_90_v2_2zhengmian.png](../img/Qddpro_PR60_x_90_v2_2zhengmian.png "fig:Qddpro_PR60_x_90_v2_2zhengmian.png") ![Qddpro_PR60_x_90_v2_2fanmian.png](../img/Qddpro_PR60_x_90_v2_2fanmian.png "fig:Qddpro_PR60_v2_2fanmian.png")
### 3D model
[Model file]( ../img/QDD_Pro-PR60-x-90_v2_2.step.zip )

## QDD Pro-PR60-80-90 Parameter

The actual reduction ratio is the harmonic nomical gear ratio plus 1 because the flexspline is fixed

<table style="width:850px"><thead><tr><th colspan="4" style="background: PaleTurquoise; color: black;">QDD Pro-PR60-80-90parameter</th></tr></thead><tbody><tr><td colspan="2" width=60%><b>Working parameters at norminal voltage</b></td><td colspan="2" width=40%><b>Mechanical parameters</b></td></tr><tr><td>Motor power</td><td>500 W</td><td>Diameter</td><td>90mm</td></tr><tr><td>Norminal voltage</td><td>42 VDC</td><td>Length</td><td>68.5mm</td></tr><tr><td>No load speed</td><td>51.9 RPM</td><td>Weight</td><td>1.23 Kg</td></tr><tr><td>Norminal speed</td><td>37 RPM</td><td>Backlash</td><td><=10 Arc sec</td></tr><tr><td>Nominal torque</td><td>26 Nm</td><td>Maximum axial load</td><td>1980N</td></tr><tr><td>Peak torque</td><td>83 Nm</td><td>Maximum radial load</td><td>1980N</td></tr><tr><td>Torque coefficient</td><td>4.9086 Nm/A</td><td>Version number</td><td>v2.2</td></tr><tr><td>Full range of phase current</td><td>33A</td><td colspan="2"><b>Work area</b></td></tr><tr><td>Nominal power current</td><td>12 A</td><td colspan="2" rowspan="15"><img src="../img/QDD Pro-6010-80-90quxian.png" style="width:300px"></td></tr><tr><td>Quiescent Current</td><td>0.08 A</td></tr><tr><td colspan="2"><b>Basic parameters</b></td></tr><tr><td>Motor type</td><td>Brushless servo motor</td></tr><tr><td>Voltage range</td><td>24~45 VDC</td></tr><tr><td>Gear ratio</td><td>81:1</td></tr><tr><td>Resolution</td><td>1327104(20bit) Step/turn</td></tr><tr><td>Encoder system</td><td>Multiturn absoulute encoder</td></tr><tr><td>Interface</td><td>Isolated CAN</td></tr><tr><td>Angle of rotation</td><td>> 360.0 °</td></tr><tr><td>Ambient temperature range</td><td>5~55 °C</td></tr><tr><td>Noise level</td><td><= 70 dB(A)</td></tr></tbody></table>


 Note: Encoder counter range: ±127turns; Motor protection temperature settable range: 25-120 °C; Inverter protection temperature settable range: 25-120 °C

### Connector Pin Layout

#### Interface specification

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="4" style="background: PaleTurquoise; color: black;width:800px">Connector Pin Layout</th></tr></thead><tbody><tr><td>Pin NO.</td><td>Color</td><td>Signal</td><td>Terminal pin distribution</td></tr><tr><td>1</td><td>PVDD</td><td>Black</td><td rowspan="9"><img src="../img/配线2-2.png" style="width:450px"></td></tr><tr><td>3</td><td>PVDD</td><td>Black</td></tr><tr><td>5</td><td>PVDD</td><td>Black</td></tr><tr><td>2</td><td>GND</td><td>Black</td></tr><tr><td>4</td><td>GND</td><td>Black</td></tr><tr><td>6</td><td>CAN-GND</td><td>Gray</td></tr><tr><td>7</td><td>CAN-L</td><td>Gray</td></tr><tr><td>8</td><td>CAN-H</td><td>Gray</td></tr></tbody></table>

## Version Updating Records

<table style="width:400px"><thead><tr style="background:PaleTurquoise"><th style="width:100px">Version number</th><th style="width:150px">Update time</th><th style="width:150px">Update content</th></tr></thead><tbody><tr><td>V2.2.0</td><td>2019.05.09</td><td>Full text added</th></tr></thead><tbody><tr><td>V1.0.0</td><td>2019.04.11</td><td>Full text added</td></tbody></table>
