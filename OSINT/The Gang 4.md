# The Gang 4

Category: OSINT

Points: 492

Solves: 74

>Can you find more information about his flight? Time to close John Doe's case one and for all! Author: NoobMaster Note: Flag format: n00bz{DateOfFlight(DD/MM/YYYY)_FlightNumber_IATAairportCodeOfDeparture_IATAairportCodeofArrival_ACTUALtimeOfDeparture_ACTUALtimeOfArrival_GateofDeparture_AirplaneModel} Example: n00bz{26/06/2024_AA1337_DFW_SFO_13:37_15:51_32A_AirbusA319-400}

### Solution

Continue where we left off (Again) (Again). First off, from the messages in `The Gang 3` we see that the flight is from `Delhi (DEL)` to `Bengaluru (BLR)`. Next we can continue to read Discord messages about flights:

![Msg1](/images/TheGang4Msgs1.png)

![Msg1](/images/TheGang4Msgs2.png)


It looks like the date of the flight is `03/08/24`, and the flight takes off at around `9:40 am` and lands around `1:00 pm`. We can now plug this information into a [flightracker](https://www.flightaware.com/live/findflight?origin=DEL&destination=BLRv). And after applying some filters we can narrow it down to `Air India 506 (AI506)`, a recurring flight from `DEL` to `BLR`. Navigating to the [August 3rd Flight](https://www.flightaware.com/live/flight/AIC506/history/20240803/0435Z/VIDP/VOBL), we get all the info we need, Some info was wrong and had to be checked with Google Flights:

![Flight](/images/TheGang4Flight.png)
![Time](/images/TheGang4Time.png)

```
Date: 03/08/2024
Flight Number: AI506
Depart Airport: DEL
Arrive Airport: BLR
Depart Time: 10:03
Arrive Time: 12:44
Depart Gate: 30
Airplane Model: AirbusA350-900
```



### Flag

```n00bz{03/08/2024_AI506_DEL_BLR_10:03_12:44_30_AirbusA350-900}```
