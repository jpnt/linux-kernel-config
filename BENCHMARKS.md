# NOTE: This tests may not be the most accurate, take everything with caution

## GPU PERFORMANCE

### GpuTest Linux - FurMark

The last digit indicates the score. Higher the better.

* i915 driver, linux 6.6.38, mesa 24.1.2, asus_wmi acpi driver
  *  FurMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.2,1920,1080,YES,Off,23,60000,0,1439

* i915 driver, linux 6.6.38, mesa 24.1.3, asus_wmi acpi driver
  *  FurMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,29,60000,0,1753

* xe driver, linux 6.9.9, mesa 24.1.2, generic acpi driver
  *  FurMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.2,1920,1080,YES,Off,12,60000,0,762

* xe driver, linux6.9.9, mesa 24.1.3, generic acpi driver
  *  FurMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,14,60000,0,876

* xe driver, linux6.9.9, mesa 24.1.3, asus_wmi acpi driver
  *  FurMark,Mesa Intel(R) Xe Graphics (TGL GT2),4.6 (Core Profile) Mesa 24.1.3,1920,1080,YES,Off,15,60000,0,953

---

## POWER CONSUMPTION

* Both systems used the same tlp.conf file. Using both asus_wmi acpi driver

* i915 driver, linux 6.6.38
  * powertop at idle: ~9.2W
  * doing furmark: ~26.8W

* xe driver, linux 6.9.9
  * powertop at idle: ~3.9W
  * doing furmark: ~17.4W

I still do not believe that we got so little power consumption with this new driver,
the performance may be a bit slower but I noticed that it was significantly cooler 
on the newer driver. If you're using a laptop and your top priority is to save battery,
then from this benchmark alone we can conclude that it is worth it a try!