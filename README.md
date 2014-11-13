PyPredict
=======

Satellite tracking and pass prediction library for Python.

PyPredict is a C Python extension directly adapted from the ubiquitous [predict](http://www.qsl.net/kd2bd/predict.html) satellite tracking tool.
We aim for result-parity and it should produce identical values on the same inputs.

If you think you've found an error, please include predict's differing output in the bug report.
If you think you've found a bug in predict, please report and we'll coordinate with upstream.
### Installation
```
sudo apt-get install python-dev
sudo python setup.py install
```
## Usage
PyPredict provides a convenient API for exposing exposes underlying functionality as well as providing a slightly convenient API.
#### Convenience API
```
import predict
tle = predict.tle(40044)
o = predict.Observer(tle)
print o.observe()
p = o.passes()
for i in range(1,10):
	np = p.next()
	print("%f\t%f\t%f" % (np.start_time(), np.end_time() - np.start_time(), np.max_elevation()))
```
#### C Implementation
```
predict.quick_find(tle.split('\n'), time.time(), (37.7727, 122.407, 25))
predict.quick_predict(tle.split('\n'), time.time(), (37.7727, 122.407, 25))
```
##API
**`Observer`**(_tle[, qth]_)  
&nbsp;&nbsp;&nbsp;&nbsp;**`observe`**(_[time]_)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Return an observation of the satellite via *quick_find(tle, time, qth)*  
&nbsp;&nbsp;&nbsp;&nbsp;**`passes`**(_[time]_)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns iterator of passes generated by *quick_predict(tle, time, qth)*  
**`PassGenerator`**(_tle[, time[, qth]]_)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Iterator returned by *Observer#passes*.  Wraps passes in *Transit* for utility methods.  
**`Transit`**(_points_)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Thin wrapper around array of observations returned by *quick_predict*.  
&nbsp;&nbsp;&nbsp;&nbsp;**`start_time`**()  
&nbsp;&nbsp;&nbsp;&nbsp;**`end_time`**()  
&nbsp;&nbsp;&nbsp;&nbsp;**`max_elevation`**()  
**`quick_find`**(_tle[, time[, qth]]_)  
&nbsp;&nbsp;&nbsp;&nbsp;_time_ defaults to now   
&nbsp;&nbsp;&nbsp;&nbsp;_qth_ defaults to latitude, longitude, altitude stored in ~/.predict/predict.qth  
&nbsp;&nbsp;&nbsp;&nbsp;Returns a dictionary containing:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*norad_id* : NORAD id of satellite.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*name* : name of satellite from first line of TLE.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*epoch* : time of observation in seconds (unix epoch)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*azimuth* : azimuth of satellite in degrees relative to groundstation.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*altitude* : altitude of satellite in degrees relative to groundstation.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*slant_rang*' : distance to satellite from groundstation in meters.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*sunlit* : 1 if satellite is in sunlight, 0 otherwise.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*decayed*: 1 if satellite has decayed out of orbit, 0 otherwise.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*geostationary* : 1 if satellite is determined to be geostationary, 0 otherwise.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*latitude* : sub-satellite latitude.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*longitude* : sub-satellite longitude.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*altitude* : altitude of satellite relative to sub-satellite latitude, longitude.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*has_aos* : 1 if the satellite will eventually be visible from the groundstation  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*doppler* : doppler shift between groundstation and satellite.  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*orbit* : refer to predict documentation  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*footprint* : refer to predict documentation  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*visibility* : refer to predict documentation  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*orbital_model* : refer to predict documentation  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*orbital_phase* : refer to predict documentation  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*eclipse_depth* : refer to predict documentation  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*orbital_velocity* : refer to predict documentation  
**`quick_predict`**(_tle[, time[, qth]]_)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Returns an array of observations (identical to those returned by `quick_find`) for the next pass.  
**`tle`**(*norad_id*)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fetch the TLE for the given NORAD id from the spire tle service.



