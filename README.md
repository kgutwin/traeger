# traeger

A simple interface to receiving updates from a Traeger WiFire grill.

## Installation

Not being on PyPI yet, you can still install via Pip:

```bash
pip install 'traeger@git+https://github.com/kgutwin/trager'
```

## Usage

In your own Python script:

```python
import traeger

username = 'your@email.com'
password = 'yourPassword'

client = traeger.traeger(username, password)

for status in client.grill_status_subscription():
    print('Grill temp:', status['status']['grill'])
    print('Probe temp:', status['status']['probe'])
    print('Pellet level:', status['status']['pellet_level'])

```

The status dictionary currently shows a substantial amount of
information:

```
{
    "thingName": "ABCD1234",
    "jobs": [],
    "status": {
        "pellet_level": 0,
        "real_time": 0,
        "time": 1679058733,
        "errors": 0,
        "sys_timer_start": 0,
        "cook_id": "ABCD1234161679051328",
        "probe": 116,
        "server_status": 1,
        "units": 1,
        "grill": 212,
        "probe_set": 205,
        "current_step": 0,
        "system_status": 6,
        "sys_timer_end": 0,
        "set": 215,
        "in_custom": 0,
        "smoke": 0,
        "cook_timer_complete": 0,
        "current_cycle": 0,
        "probe_alarm_fired": 0,
        "ambient": 71,
        "probe_con": 1,
        "sys_timer_complete": 1,
        "cook_timer_start": 0,
        "cook_timer_end": 0,
        "keepwarm": 0,
        "connected": true,
        "acc": [
            {
                "uuid": "p0",
                "channel": "p0",
                "type": "probe",
                "con": 1,
                "probe": {
                    "get_temp": 116,
                    "set_temp": 205,
                    "alarm_fired": 0
                }
            }
        ]
    },
    "features": {
        "pellet_sensor_enabled": 1,
        "pellet_sensor_connected": 0,
        "open_loop_mode_enabled": 0,
        "cold_smoke_enabled": 0,
        "grill_mode_enabled": 0,
        "time": 1679051228,
        "super_smoke_enabled": 0
    },
    "limits": {
        "max_grill_temp": 500
    },
    "settings": {
        "rssi": -42,
        "units": 1,
        "time": 1679051228,
        "config_version": "10.003",
        "feature": 0,
        "speaker": 1,
        "device_type_id": 2110,
        "language": 0,
        "fw_version": "02.02.01",
        "ssid": "ABCD1234",
        "fw_build_num": "20201204.120225"
    },
    "usage": {
        "auger": 569844,
        "grill_clean_countdown": 3962,
        "time": 1679058400,
        "error_stats": {
            "bad_thermocouple": 0,
            "ignite_fail": 0,
            "auger_ovrcur": 0,
            "overheat": 0,
            "lowtemp": 1,
            "auger_disco": 0,
            "low_ambient": 0,
            "fan_disco": 0,
            "ign_disco": 0
        },
        "fan": 905030,
        "runtime": 909155,
        "hotrod": 51495,
        "grease_trap_clean_countdown": 0,
        "cook_cycles": 126
    },
    "custom_cook": {
        "cook_cycles": [
            {
                "cycle_name": "Chicken Challenge",
                "populated": 1,
                "num_steps": 1,
                "units": 1,
                "slot_num": 0,
                "steps": [
                    {
                        "probe_set_temp": 170,
                        "time_set": 0,
                        "keepwarm": 0,
                        "smoke": 0,
                        "step_num": 1,
                        "set_temp": 425,
                        "use_timer": 0
                    }
                ]
            },
            {
                "cycle_name": "Beginner's Brisket",
                "populated": 1,
                "num_steps": 2,
                "units": 1,
                "slot_num": 1,
                "steps": [
                    {
                        "probe_set_temp": 0,
                        "time_set": 12600,
                        "keepwarm": 0,
                        "smoke": 0,
                        "step_num": 1,
                        "set_temp": 165,
                        "use_timer": 1
                    },
                    {
                        "probe_set_temp": 190,
                        "time_set": 0,
                        "keepwarm": 0,
                        "smoke": 0,
                        "step_num": 2,
                        "set_temp": 225,
                        "use_timer": 0
                    }
                ]
            },
            {
                "slot_num": 2,
                "populated": 0
            },
            {
                "slot_num": 3,
                "populated": 0
            },
            {
                "slot_num": 4,
                "populated": 0
            }
        ]
    },
    "stateIndex": 49362,
    "schemaVersion": "1.0",
    "details": {
        "thingName": "ABCD1234",
        "userId": "a4fcd574-ed56-4eb3-bdbc-ba52ee08e4e5",
        "lastConnectedOn": 1679012255,
        "thingNameLower": "abcd1234",
        "friendlyName": "Grill",
        "lat": 20.12345
        "long": -70.12345,
        "deviceType": "2110"
    }
}
```

## Origin

From the original author's notes:

> I wish traeger would just have an open API, this might break at any
> time though I think using a lot of AWS' IOT tools might limit that
> risk.  I reversed the traffic from the android app mainly and also
> looked at the AWS IOT docs to figure out the usage.  I couldn't find
> the 'shadow' data in AWS for a one time lookup so I'm subscribing and
> waiting for data which is pretty lame.  I haven't had time to make it
> better recently so I'm going to share this for now.

> The traeger2graphite script is what I'm currently using to get a graph
> of my smoker data and is a simple example too.  Needs a simple
> ~/.traeger config file with the contents:

> `{"username": "<your email>", "password": "<your password", "graphite_port": "2003", "graphite_host": "graphite.local"}`
