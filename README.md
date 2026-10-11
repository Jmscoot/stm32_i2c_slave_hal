# I2C 통신 테스트 (C2000 Master ↔ STM32 Slave)

C2000(I2C-A, Master)이 10ms 주기로 STM32(I2C1, Slave)에서 2바이트를 읽는 테스트 프로젝트.

## 설정 요약

| 항목 | Master (TI C2000) | Slave (STM32F446RE) |
|---|---|---|
| 역할 | Controller (Receive) | Target (Transmit) |
| I2C 모듈 | I2C-A | I2C1 |
| SDA 핀 | GPIO104 (SDAA) | PB7 (AF4) |
| SCL 핀 | GPIO105 (SCLA) | PB6 (AF4) |
| 핀 설정 | 내부 Pull-up, Async qualification | Open-drain, No pull |
| Slave 주소 (7bit) | 0x50 | 0x50 (`OwnAddress1 = 0x50 << 1`) |
| 전송 속도 | 400 kHz (Fast-mode) | 400 kHz (Fast-mode) |
| Duty cycle | 33% (`I2C_DUTYCYCLE_33`) | 2 (Tlow/Thigh = 2) |
| SYSCLK | 200 MHz (`DEVICE_SYSCLK_FREQ`) | 16 MHz (HSI, PLL 미사용) |
| I2C 모듈 클럭 | SYSCLK 기반 | PCLK1 = 16 MHz (APB1 /1) |
| 전송 바이트 수 | 2 byte Read | 2 byte (`0x12`, `0x34`~ 증가) |
| 통신 주기 | 10 ms | 요청 시 응답 (Polling, timeout 1s) |

## 결선

| C2000 | STM32 |
|---|---|
| GPIO104 (SDA) | PB7 (SDA) |
| GPIO105 (SCL) | PB6 (SCL) |
| GND | GND |

- SDA/SCL에 외부 Pull-up 1kΩ (@ 3.3V)  
- 두 보드 GND 공통 연결 필수
