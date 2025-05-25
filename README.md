# Retentive-Timer-Motor-Control-
This RSLogix 500 ladder logic program uses a **retentive timer (RTO)** to monitor total motor runtime across multiple start/stop cycles. The motor will automatically shut off once 10 seconds of cumulative operation is reached, and the system can be manually reset to restart the process.
---

## 🧠 Core Features

- One-shot (`ONS`) logic to prevent trigger latching on hold
- `RTO` (Retentive Timer) to track time across multiple cycles
- `RES` for full reset of `.ACC` and `.DN`
- Interlocked motor output with safe start/stop logic
- Parallel structure for timer and output activation

---


🔧 Ladder Logic Breakdown 
### 🔁 Rung 0 – Start Button One-Shot ladder

[ B3:0/0 ] ----[ONS B3:0/4]---- ( B3:0/5 )
The Start button triggers TIMER TRIGGER (B3:0/5) via a one-shot

Ensures trigger is latched only once per press

### 🔁 Rung 1 – Timer and Motor Logic (Parallel Activation) ladder

BST 
  [ B3:0/5 ]   ; Start trigger is latched
NXB
  [ T4:0/EN ]  ; Timer is already running
[ XIO B3:0/2 ] ; Stop button NOT pressed
[ XIO T4:0/DN ] ; Timer not done
BND

BST
  [ RTO T4:0 ]
    Time Base: 1.0s
    Preset: 10
NXB
  ( B3:0/1 )   ; Motor ON
BND
This rung does two things in parallel:

Runs the RTO timer

Turns the motor ON while the timer is running and valid

The logic ensures the motor shuts OFF:

If Stop is pressed

Or timer is done

Or before the cycle begins

### 🔁 Rung 2 – Manual Reset ladder

[ B3:0/3 ] ---- ( RES T4:0 )
Pressing the Reset button clears:

.ACC (accumulated time)

.DN (done bit)

### 🔚 Rung 3 – END ladder

END

🛠 Tools Used
RSLogix Micro Starter Lite

RSLinx Classic

RSLogix Emulate 500
