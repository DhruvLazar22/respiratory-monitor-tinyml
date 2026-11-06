# Embedded Respiratory Monitoring System (ESP32 + TinyML)

A portable device that listens to breathing sounds and classifies them as **normal, cough or wheeze** in real time. Everything runs **on the ESP32 itself**, with no internet or cloud, which keeps it fast, cheap and private.

**Course:** Embedded C Programming (BECE320E), VIT Vellore
**Team:** Anna Maria, Dhruv Lazar
**Tools:** ESP32, MAX9814 microphone, TensorFlow Lite Micro, Embedded C

---

## Pipeline

```
MAX9814 mic → 12-bit ADC @ 8 kHz → band-pass 100–2500 Hz → noise reduction + normalisation
     → 25 ms frames (Hamming window) → MFCC features → CNN-LSTM (TFLite Micro on ESP32)
     → Normal / Cough / Wheeze → LED + buzzer alert
```

| Stage | Details |
|---|---|
| Sensing | MAX9814 microphone, with built-in amplifier and automatic gain control |
| Sampling | 12-bit ADC, 8 kHz (covers the 100 to 2500 Hz range of lung sounds) |
| Features | MFCCs from 25 ms frames with a Hamming window |
| Model | **CNN-LSTM hybrid**. The CNN picks up spectral patterns and the LSTM tracks how breathing changes over time. |
| Deployment | TensorFlow Lite Micro on the ESP32 |
| Output | LEDs + buzzer when an abnormal sound is detected |

## Why on-device?

Most respiratory monitors send audio to the cloud. Doing it on the device means:
- **Low latency:** alerts arrive instantly.
- **Privacy:** audio never leaves the device.
- **Works offline:** useful in remote or low-resource clinics.

## Results

The prototype captured breathing sounds continuously, classified them in real time, and raised alerts for cough and wheeze events.

**SDG alignment:** SDG 3, Good Health and Well-Being.

## Future work

- Larger training dataset
- More sound classes (e.g. crackles)
- Wireless link for remote monitoring

## References

1. B. M. Rocha et al., "Automatic classification of lung sounds," *Physiological Measurement*, 2019.
2. G. Petmezas et al., "Automated lung sound classification using a hybrid CNN-LSTM network," *Sensors*, 2022.
