# 🔧 FreeRTOS-STM32F446RE

> Embedded systems experiments on **STM32F446RET6** using **FreeRTOS** and **STM32CubeIDE**  
> From bare-metal GPIO to multi-task RTOS scheduling — all in one place.

---

## 🛠️ Hardware & Tools

| Item | Details |
|------|---------|
| MCU | STM32F446RET6 |
| Core | ARM Cortex-M4 @ 180 MHz |
| Flash | 512 KB |
| RAM | 128 KB |
| RTOS | FreeRTOS (via STM32CubeIDE middleware) |
| Toolchain | STM32CubeIDE + STM32CubeMX |
| Board | Nucleo-F446RE |

---

## 📁 Experiments Overview

| # | Title | Concepts |
|---|-------|---------|
| 01 | GPIO Digital Output — LED Blink | GPIO Output, Software Delay |
| 02 | GPIO Digital Input — Push Button LED Toggle | GPIO Input, Debouncing |
| 03 | HC-SR04 Ultrasonic Sensor — Distance Classification | GPIO, Timer, USART, Sensor Interfacing |
| 04 | PWM LED Brightness Control | Timer, PWM, Duty Cycle |
| 06 | FreeRTOS — Dual Task Priority Analysis | FreeRTOS, Task Priority, Preemption |
| 07 | FreeRTOS — Dual Task Priority with SWV ITM Tracing | FreeRTOS, CMSIS-V2, SWV ITM Console |
| 08 | FreeRTOS — Binary Semaphore with EXTI Button Interrupt | FreeRTOS, Binary Semaphore, EXTI |

---

## 🔬 Experiment Details

---

### 01 — GPIO Digital Output (LED Blink)

**AIM:** Configure a GPIO pin of STM32F446RE as digital output and verify LED blinking operation using software delay routines.

- Configured **PA5** as GPIO Output
- Used `HAL_Delay()` for software delay
- Onboard LED (LD2) blinks at a defined interval

---

### 02 — GPIO Digital Input (Push Button LED Toggle)

**AIM:** Interface a push button as digital input and demonstrate LED control by toggling its state on each valid button press.

- Configured **PC13** as GPIO Input (User Button)
- LED toggles state on each valid button press
- Includes basic debounce handling

---

### 03 — HC-SR04 Ultrasonic Sensor with USART

**AIM:** Interface an HC-SR04 ultrasonic sensor with STM32F446RE and classify distance ranges using visual indication through LEDs, with output on serial monitor via USART.

- Trigger pulse generated via GPIO Output
- Echo pulse duration measured using Timer input capture
- Distance classified into ranges:
  - 🟢 **Close** → Green LED ON
  - 🟡 **Medium** → Yellow LED ON
  - 🔴 **Far** → Red LED ON
- Distance values printed via **USART2 @ 115200 baud**

---

### 04 — PWM LED Brightness Control

**AIM:** Generate a PWM signal using a timer on STM32F446RE and control the brightness of an onboard LED by varying the duty cycle.

- Timer configured in **PWM mode**
- Duty cycle varied from 0% → 100% → 0% (breathing effect)
- Onboard LED brightness changes smoothly

---

### 06 — FreeRTOS Dual Task Priority Analysis

**AIM:** Create and execute two FreeRTOS tasks with different priorities and analyze their effect on LED blinking behaviour.

- Two tasks created with **different priorities**
- Higher priority task preempts lower priority task
- LED blinking rates differ based on task priority
- Demonstrates FreeRTOS **preemptive scheduling** behaviour

---

### 07 — FreeRTOS Dual Task Priority with SWV ITM Tracing

**AIM:** Create and execute two FreeRTOS tasks with different priorities and analyze their effect on LED blinking behaviour using SWV ITM Data Console.

- Two tasks (`LED_1` and `LED_2`) created with **CMSIS-RTOS v2** interface
- Priority levels tested: `osPriorityLow`, `osPriorityNormal`, `osPriorityHigh`, `osPriorityRealtime`
- `printf` retargeted to **SWV ITM Console** via `ITM_SendChar()`
- Demonstrates **CPU starvation** of lower-priority tasks
- LED blink rates and ITM console message frequency observed on **Port 0**

---

### 08 — FreeRTOS Binary Semaphore with EXTI Button Interrupt

**AIM:** Configure an external interrupt (EXTI) for a user button and use a binary semaphore to synchronize an LED control task.

- Implements **Deferred Interrupt Processing** — ISR stays short, logic moved to task
- **PC13** configured as EXTI with **Falling Edge Trigger** (NVIC priority: 7)
- Binary semaphore released by ISR on button press
- `LED_Control` task blinks LED **5 times** (250 ms ON/OFF) on each press
- `printf` retargeted to **SWV ITM Console** for trace output

**Application Flow:**
```
Button Press → EXTI ISR → osSemaphoreRelease()
                              ↓
                    LED_Control task unblocks
                              ↓
                    LED blinks 5× (250 ms each)
                              ↓
                    Task blocks again (waits for next press)
```

---

## 🚀 Getting Started

### Prerequisites
- [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html) v1.13+
- ST-Link V2 or onboard debugger
- STM32F446RET6 Nucleo board

### Clone the Repo
```bash
git clone https://github.com/krrish4556ece24-lang/FREE_RTOS.git
```

### Opening a Project in STM32CubeIDE
1. Open **STM32CubeIDE**
2. Go to `File` → `Import` → `General` → `Existing Projects into Workspace`
3. Browse to any experiment folder
4. Click **Finish**
5. Build: `Ctrl+B` → Flash & Debug: `F11`

---

## 📌 Pin Reference

| Pin | Function |
|-----|---------|
| PA5 | Onboard LED (LD2) |
| PA6 | External LED (Exp 07) |
| PC13 | User Push Button / EXTI Input |
| PA2 | USART2 TX (Serial Monitor) |
| PA3 | USART2 RX |

---

## 📄 License

MIT License — free to use, modify, and share.

---

## 🙋 Author

**Krrish Popli**  
GitHub: [@krrish4556ece24-lang](https://github.com/krrish4556ece24-lang)
