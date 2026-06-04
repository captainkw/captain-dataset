# Captain Flight Dataset

Open flight telemetry from a real Cirrus SR22 Turbo (Garmin Perspective+) general-aviation flight, released as an open dataset for building, testing, and benchmarking telemetry tooling.

## Flight

- Aircraft:  Cirrus SR22 Turbo (3600 GW), Garmin Perspective+ avionics.
- Route:  Santa Monica (KSMO) to Santa Maria (KSMX), California.
- Date:  2026-03-11.
- Span:  15:22:46 to 16:33:06 local (~70 minutes, gate to gate), 4,066 samples at roughly 1 Hz.

## Contents

- `flight_dataset/SMO-SMX-full-flight-garmin-perspective-raw-3-11-2026.csv` — the raw Garmin Perspective+ flight log export, unmodified (71 columns).

## Format

Standard Garmin flight-log CSV.  The first three lines are headers, then one data row per sample:

1. `#airframe_info, ...` — airframe and avionics software metadata.
2. `#yyy-mm-dd, hh:mm:ss, ...` — the physical unit for each column.
3. `Lcl Date, Lcl Time, ...` — the column names.

Empty cells mean the value was not recorded for that sample.

## Channels (selected, 71 total)

| Group | Columns |
|-------|---------|
| Time / position | Lcl Date, Lcl Time, UTCOfst, Latitude, Longitude |
| Altitude | AltB, BaroA, AltMSL, AltGPS |
| Speed | IAS, TAS, GndSpd, VSpd |
| Attitude | Pitch, Roll, HDG, TRK, LatAc, NormAc |
| Electrical | volt1, volt2, amp1 |
| Fuel | FQtyL, FQtyR, E1 FFlow |
| Engine | E1 OilT, E1 OilP, E1 MAP, E1 RPM, E1 %Pwr, E1 CHT1-6, E1 EGT1-6, E1 TIT1-2 |
| Nav / radio | CRS, NAV1, NAV2, COM1, COM2, HCDI, VCDI |
| Wind / GPS integrity | WndSpd, WndDr, GPSfix, HAL, VAL, HPLwas, HPLfd, VPLwas |

## Usage

Fetch the raw CSV directly:

```
https://raw.githubusercontent.com/captainkw/captain-dataset/main/flight_dataset/SMO-SMX-full-flight-garmin-perspective-raw-3-11-2026.csv
```

To ingest it:

1. Skip line 1 (airframe metadata), read line 2 as units and line 3 as column names, then parse the data rows.
2. Build each sample's timestamp from `Lcl Date` + `Lcl Time` + `UTCOfst` (e.g. `2026-03-11T15:22:46-07:00`).
3. Treat the remaining columns as channels.  62 are numeric; skip the text/enum columns (`AtvWpt`, `HSIS`, `RollM`, `PitchM`, `GPSfix`, `VAL`).
4. Empty cells mean no sample for that channel at that time; forward-fill if your sink needs equal-length series.

## Intended use

A small, real, fully-labeled flight log to use as a golden dataset.

## License

MIT.  See [`LICENSE`](LICENSE).  Free to use, modify, and redistribute, including commercially, with attribution.

## Attribution

If you use this dataset, please credit "Captain Flight Dataset, Kuangwei Hwang".
