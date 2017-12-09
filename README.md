# CONUS-RTT
Continental US Network RTT-based distance estimates vs. Great Circle distance (ground truth)

After reading Brian Trasmmell's:
https://github.com/britram/trilateration/blob/master/paper.ipynb
I repeated some of
the calculations independently, using a dataset from a different
measurement platform.

Using the WIPM measurement system I co-designed back in ~2000,
our early design & results from IPPM@IETF-50: 
https://www.ietf.org/proceedings/50/slides/ippm-3.pdf

and the sort of data AT&T makes public:
https://ipnetwork.bgtmo.ip.att.net/pws/network_delay.html

I obtained the lat/long for our 27 CONUS nodes with 
systems that conduct a full mesh of RTT measurements
(but we consider A -> B -> A the same as B -> A -> B).
The RIPE Atlas data was sparse in the US, so this is a 
complimentary geographic area to Brian's dataset.

As the slides from IETF-50 show,
we run the same set of tests every 15 minutes over the full mesh. 
From 30 minutes of results, I selected 689 RTT measurements 
marked for the Best Effort class.

The minimum RTT is calculated from ~280 Poisson-distributed packets
(average inter-packet interval = 3.3 seconds).
Dapp = 0, because we have 4 timestamps with resolution = 1ms
(and this resolution alone is a source of error, as Brian pointed out,
yet this amount of error has been inconsequential for network monitoring).
There are other packet distributions and classes in this dataset, 
a total of 9036 measurements (only 689 described above are plotted).

I reproduced some of Brian's analysis in a spread sheet, 
and made a plot that’s similar to Brian's Figure 3.

URL.pdf

The blue points are the measured RTT/2, and the green line
is the Great Circle Route RTT/2 delay using the same constants
(fiber refraction = 1.4677, and mean Earth radius = 6371km).
I didn't plot the orange line, and Brian plotted RTT directly.
So, this is mostly a confirmation of what Brian already accomplished,
extended to US locations. For WIPM data, I believe that the difference 
between the actual network route and the great circle distance accounts
for most of the meas-ideal delay error, owing to the location
of the measurement devices in backbone nodes (no access links), 
and the small delay variation over ~280 packets 
(queue and some processing times vary). 

I also examined the implications of these errors on a map. IIRC, 
DKG indicated that localization to a state would be enough 
for some purposes. It turns-out that 3ms or 300km error 
is enough error to make most US state locations ambiguous (which is 
the lion’s share of these single measurement errors, >85%), but 1ms or 100km
error would be sufficient to exclude other states, depending on
the estimated location and the particular state. 
However, only 27 of 689 have one way meas-ideal < 1ms.


