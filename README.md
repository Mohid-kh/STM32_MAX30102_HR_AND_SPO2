MAX30102 + STM32F401RE — Heart Rate (BPM), SpO₂, Temperature
This project shows how to read IR/RED PPG from MAX30102 with STM32F401RE, compute heart rate (BPM), estimate SpO₂, and read die temperature.

Hardware
MCU: Nucleo-F401RE

Sensor: MAX30102

Communication Protocol : I2C protocol

Firmware (STM32CubeIDE)
I²C1 @ 300 kHz

TIM1 interrpt (1s) for serial monitoring of data

MAX30102 config (typical):

Mode: SpO₂ or HR

Pulse width: 411 µs (18-bit)

Sample rate: 100 Hz

FIFO avg: 4–8

LED current: start ~6–12 mA, balance IR slightly higher than RED

Algorithms (brief)
Heart Rate (BPM):

DC removal: AC = IR - EMA(IR)

Peak detect on AC ⇒ inter-beat interval (IBI)

BPM = 60000 / IBI_ms (optionally average last 3–5 beats)

SpO₂ (ratio-of-ratios, heuristic):

R = (ACred/DCred) / (ACir/DCir)

SpO₂ ≈ 110 − 25·R (tune constants per hardware; smooth over a few beats)

Temperature: trigger TEMP_EN, wait for DIE_TEMP_RDY, read TINT + TFRAC (×0.0625°C)

Build & Run
Open the project in STM32CubeIDE, generate code, build.

Flash the board.

View live values via UART or SWV (Live Expressions).

Place fingertip or earlobe steadily on sensor; avoid motion.

Notes
Real-world SpO₂ needs calibration (constants vary by sensor, optics, skin tone).

Use EMA / band-pass filters and beat gating for stable numbers.

Temperature is sensor die temperature (for compensation), not body temp.
