## Project: Software-Triggered Interrupts using NVIC (`ISPR` & `STIR`) on ARM Cortex-M4

## Objective: 

A bare-metal C project for STM32 (ARM Cortex-M4) demonstrating software-generated interrupts using the **Nested Vectored Interrupt Controller (NVIC)** registers without requiring physical external hardware signals.

The project periodically toggles an LED on pin **PA5** by forcing **EXTI0** and **EXTI1** into pending states using `NVIC_SetPendingIRQ()` (`ISPR`) and `NVIC->STIR`, timed with a custom **SysTick** millisecond delay driver.

---

## 📌 Features

* **Software Interrupt Generation:**
  * Triggers `EXTI0_IRQHandler` using `NVIC_SetPendingIRQ(EXTI0_IRQn)` to set **PA5 HIGH** (LED ON).
  * Triggers `EXTI1_IRQHandler` using direct write to `NVIC->STIR = EXTI1_IRQn` to set **PA5 LOW** (LED OFF).
* **Precise Hardware Timing:** Implements a non-blocking `delay_ms()` function using the ARM Cortex-M **SysTick** timer.
* **Bare-Metal Register Access:** Direct manipulation of GPIO registers (`RCC->AHB1ENR`, `GPIOA->MODER`, `GPIOA->ODR`).

---

## 🛠 Hardware & Software Requirements

* **Microcontroller:** STM32F4 series (e.g., NUCLEO-F446RE, NUCLEO-F401RE, or STM32F4 Discovery)
* **IDE / Toolchain:** Keil uVision MDK-ARM (v5.x or higher) / Arm GNU Toolchain
* **Debugger:** ST-Link V2/V3
* **Onboard Hardware:** User LED connected to **PA5**


[ main() Starts ]
              │
   Configure GPIOA Pin 5 (Output)
              │
   Configure & Enable SysTick Timer
              │
   Set Priorities & Enable NVIC IRQs
              │
    ┌─────────┴─────────┐
    │    Infinite Loop  │ ◄─────────────────────────┐
    └─────────┬─────────┘                           │
              │                                     │
 1. Trigger EXTI0 via ISPR ────► EXTI0_IRQHandler() │
    (Set Pending Bit)            └─► Set PA5 HIGH   │
              │                                     │
 2. Delay 1000 ms via SysTick                       │
              │                                     │
 3. Trigger EXTI1 via STIR ────► EXTI1_IRQHandler() │
    (Software Trigger Register)  └─► Set PA5 LOW    │
              │                                     │
 4. Delay 1000 ms via SysTick                       │
              └─────────────────────────────────────┘


