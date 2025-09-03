# PWM
Pulse Width Modulation (PWM) is a technology that uses a on-off time of a (periodic) signal to convey information.

We mainly use it to control **servo motors** and DC motors.

## Principle and Terms
REF: ELEC1100 lecture 10 (2022, Deparment of Electronic and Computer Engineering, HKUST) ![](https://i.imgur.com/jnZB3Kd.jpg) ![](https://i.imgur.com/7Iyfnpg.jpg)

## Generating PWM
There are 2 main components when generating a PWM
- Frequency (1 / T)
- On-time (V HIGH time)
  - or Duty Cycle (On-time / T)

In the following parts, we will try to generate a PWM with 50Hz frequency and 2ms on-time.

### Frequency: MCU Clock & Timer 
The MCU Clock is what determines how fast a MCU (/CPU) can operate.

For our boards, they are usually at 84MHz (84 000 000 instructions in a second!).\
But for our purpose, we only need 50Hz.

So we will be using **timers** inside the board to generate a different frequency.
*Note* a total of 8 timers (TIM0 - TIM8) are available, each can output different frequencies.

#### TIM
Each TIM has some custom parameters that we could set, and we will be using
```c
uint16_t PSC; // Prescaler Value
uint16_t ARR; // Auto Reload Register
```
to slow down the frequency.

- PSC: how many **MCU Clock cycles** to wait before increasing **auto-reloaded counter**\
  i.e. PSC = 1, output is send every (PSC + 1) MCU Clock cycles: 1st MCU clk cycle wait, 2nd MCU cycle output
- ARR: max value of auto-reloaded counter\
  i.e. ARR = 1, *counter overflow* triggered when *auto-reloaded counter* **exceeds** ARR value, that is every (ARR + 1) *auto-reloaded counter* counts\
  and it would be **reset to 0** after overflow.

>Mainly refer to those items in red
>
![](https://imgur.com/YF8xjhF.png)

**ClassWork**
- [ ] find 3 pairs of PSC and ARR values to generate a 50Hz output frequency from our board (84GHz).
### On-time: Channels & CRR
