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

* **Microcontroller:** STM32 NUCLEO-F446RE
* **IDE / Toolchain:** Keil uVision MDK-ARM / Arm GNU Toolchain
* **Debugger:** ST-Link
* **Onboard Hardware:** User LED connected to **PA5**



