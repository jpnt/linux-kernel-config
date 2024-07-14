# NOTE: Tests may not be the most accurate, take everything with caution

## GPU PERFORMANCE

### GpuTest Linux

* xe driver, linux 6.9.9, mesa 24.1.3

Module,Renderer,ApiVersion,Width,Height,Fullscreen,AntiAliasing,AvgFPS,Duration,MaxGpuTemp,Score
GiMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,24,60000,0,1484
Plot3D,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,121,60000,0,7289
PixMark Piano,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,11,60000,0,711
Triangle,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,410,60000,0,24629
FurMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,16,60000,0,967

* i915 driver, linux 6.9.9, mesa 24.1.3

Module,Renderer,ApiVersion,Width,Height,Fullscreen,AntiAliasing,AvgFPS,Duration,MaxGpuTemp,Score
GiMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,28,60000,0,1693
Plot3D,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,178,60000,0,10728
PixMark Piano,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,13,60000,0,796
Triangle,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,353,60000,0,21194
FurMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,30,60000,0,1848

From this benchmarks, we can see that the new driver still has optimizations to do, because the old drivers outperform it
in the majority of tests, except the triangle benchmark where the avgfps was 410 xe and 353 on i915.

---

## POWER CONSUMPTION

* Both systems used the same tlp.conf file. Using both asus_wmi acpi driver. Powertop was used to report power usage.

* xe driver, linux 6.9.9, mesa 24.1.3
  * idle: 6W
  * gimark: 13W
  * plot3d: 11.8W
  * pixmark piano: 14.3W
  * triangle: 12.8W
  * furmark: 17.7W

* i915 driver, linux 6.9.9, mesa 24.1.3
  * idle: 6W
  * gimark: 14W
  * plot3d: 13.3W
  * pixmark piano: 14.3W
  * triangle: 11.5W
  * furmark: 23.6W

From what I could gather, I see that the newer driver is more power efficient, meaning consumes less power. However
this could also mean that its not using the gpu to its full capabilities resulting in less use = less power used (this
can be seen in the triangle test).

Nonetheless, this driver shows great promise, achieving good performance, despite being very new.
