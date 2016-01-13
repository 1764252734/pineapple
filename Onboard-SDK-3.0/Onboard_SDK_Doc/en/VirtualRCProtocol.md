# Virtual RC

## Introduction

Virtual RC (Remote Controller), is an API designed for developers to control the drone through serial port by simulated channel values.

Developers can use it directly without obtaining control ability. A list of channel data should be sent to the drone from serial port within every second, or the drone will exit from the virtual RC logic.

## Command Set and Command ID

|API LEVLE|CMD SET|CMD ID|DESCRIPTION|
|---------|-------|------|-----------|
|0|0x05|0x00|obtain of release virtual RC control|
|0|0x05|0x01|virtual RC channel data|


## Data Structure

```c
/* Struct of RC Enable/Disable */
typedef struct
{
    uint8_t on_off: 1; //open (1) or close (0) the virtual RC
    uint8_t if_switch_back_to_real_RC: 1; //if switch back to real RC (1) or run RC-lost logic directly (0)
    uint8_t reserved: 6; //reserved
}virtual_rc_enable;

/* Struct of RC Data */
typedef struct
{
    uint32_t rc_raw_data[16]; //developer designed channel value
}virtual_rc_data
```

## Note

1. The drone will exit *virtual RC* logic if there is a whole second your drone does not receive any `virtual_rc_data`. After exiting the *virtual RC*, if `if_switch_back_to_real_RC` is `1`, the drone will switch back to the real RC; if `if_switch_back_to_real_RC` is `0`, the drone will run into the *RC lost* logic directly, which is the same as the one when you lost your **real RC** connection.

2. The specific meaning of each channel is defined by developers himself and can be configured in DJI assistant.

    The default one for DJI controller is defined as follows: 

    |Function Channel|Physical Channel|Channel Type|Range|
    |------|-------|-------|---|
    |Roll|0|Joystick|[1024-660, 1024+660]|
    |Pitch|1|Joystick|[1024-660, 1024+660]|
    |Throttle|2|Joystick|[1024-660, 1024+660]|
    |Yaw|3|Joystick|[1024-660, 1024+660]|
    |MODE|6|Third gear switch|1552(P),1024(A),496(F)|
    |GO_HOME|5|Button|N/A|
    |GEAR|4|Second gear switch|1684(Up),1324(Down)|