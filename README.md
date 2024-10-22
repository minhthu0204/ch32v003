# CH32V003
## Tổng quan

CH32V003 là vi điều khiển 32-bit dựa trên kiến trúc RISC-V với lõi xử lý QingKe, được phát triển bởi WCH. Nó có xung nhịp tối đa 48 MHz, kèm theo 16KB bộ nhớ flash và 2KB SRAM, rất phù hợp cho các hệ thống nhúng nhỏ gọn và các ứng dụng điều khiển thời gian thực.

### Sơ đồ khối hệ thống
![enter image description here](https://github.com/openwch/ch32v003/raw/main/image/frame.jpg)

### Thông số chính:
- **Bộ nhớ**: 2KB SRAM và 16KB Flash.
- **Điện áp cung cấp**: Hỗ trợ hoạt động ở mức **3.3V hoặc 5V**.
- **Chế độ tiết kiệm năng lượng**: Tích hợp các chế độ **Ngủ và Chờ**, giúp tối ưu hóa mức tiêu thụ năng lượng.
- **Khởi động an toàn**: Hỗ trợ khởi động lại khi bật/tắt nguồn và có bộ phát hiện điện áp có thể lập trình được.
- **DMA đa dụng**: 1 kênh **bộ điều khiển DMA** giúp chuyển dữ liệu mà không cần CPU can thiệp.
- **Công cụ tích hợp**: 1 nhóm **so sánh khuếch đại** và 1 nhóm **ADC 10-bit** hỗ trợ thu thập và xử lý tín hiệu nhanh chóng.

### Các tính năng mở rộng:
- Bộ định thời: Gồm 1 bộ định thời điều khiển nâng cao **16-bit** và 1 bộ định thời đa dụng **16-bit**.
- Các tính năng bảo vệ hệ thống: Tích hợp **2 bộ giám sát WDOG** và **1 bộ SysTick 32-bit** giúp đảm bảo hệ thống hoạt động ổn định.
- Giao diện truyền thông: Bao gồm **1 giao diện USART**, **1 nhóm I2C**, và **1 nhóm SPI**.

Ngoài ra, chip còn sở hữu **18 cổng I/O** có khả năng ánh xạ ngắt ngoài, **ID duy nhất 64-bit** và tích hợp **giao diện debug 1 dây (SDI)** để dễ dàng phát triển và sửa lỗi. Dòng CH32V003 có nhiều tùy chọn đóng gói nhỏ gọn như **TSSOP20, QFN20, SOP16, SOP8**, giúp tối ưu không gian thiết kế.

## Các board trên thị trường
### CH32V003F6P6 V1227 
![enter image description here](https://down-vn.img.susercontent.com/file/sg-11134201-7rcbz-lsqqop6686zq95.webp)

>  😓 Em khong tìm được tên board này và schematic của nó, cái gần giống nhất em tìm được trên hãng là https://github.com/wuxx/nanoCH32V003

### CH32V003F4P6-R0-1v1
![enter image description here](https://github.com/openwch/ch32v003/raw/main/SCHPCB/CH32V003F4P6-R0-1v1/image/board1.jpg)

### Schematic
![enter image description here](https://ch405-labs.com/content/images/2023/11/CH32V00xSCH.png)

- **Power**: Chuyển đổi nguồn 5V xuống 3.3V qua IC ổn áp 6219P33M.
- **MCU**: Vi điều khiển CH32V003F4P6, hỗ trợ nhiều chân I/O và hoạt động với 3.3V/5V.
- **LED**: LED1 và LED2 kết nối với chân PC0 và PC1, qua điện trở 1kΩ.
- **USB**: Giao diện USB Type-C với các điện trở kéo 5.1kΩ.
- **PIN**: Hai header 7x2 kết nối với chân I/O của MCU.
- **RST**: Mạch reset đơn giản với nút nhấn và tụ điện.
- **OPA**: Mạch so sánh khuếch đại kết nối với chân PA1, PD0, và PD4.

### Chức năng chính của các chân GPIO

 **GPIOA (PA1 - PA2)**
   - **Chức năng chính:**
     - **Dao động thạch anh ngoài (Oscillator):** PA2 (OSCO) và PA1 (OSCI) dùng cho dao động thạch anh để tạo xung nhịp chính xác.
     - **Chuyển đổi tín hiệu analog sang digital (ADC):** PA1 (A1), PA2 (A2) dùng để lấy tín hiệu từ các cảm biến hoặc nguồn tương tự.
     - **Timer & PWM:** PA1, PA2 hỗ trợ điều khiển Timer 1, bao gồm các kênh Timer để đo thời gian hoặc tạo tín hiệu PWM cho ứng dụng điều khiển động cơ.
     - **Bộ khuếch đại hoạt động (OP):** PA1 (OPP0) và PA2 (OPN0) dùng cho xử lý tín hiệu tương tự với bộ khuếch đại.

**GPIOC (PC0 - PC7)**
   - **Chức năng chính:**
     - **Giao tiếp UART:** PC0 (UTX), PC1 (URX), PC2 (URTS), PC3 (UCTS) hỗ trợ truyền và nhận dữ liệu qua UART.
     - **Giao tiếp I2C:** PC1 (SDA) và PC2 (SCL) dùng cho giao tiếp I2C để trao đổi dữ liệu với các thiết bị ngoại vi như cảm biến.
     - **Giao tiếp SPI:** PC0 (NSS), PC5 (SCK), PC6 (MOSI), PC7 (MISO) hỗ trợ giao tiếp SPI để truyền dữ liệu nhanh giữa vi điều khiển và thiết bị ngoại vi (ví dụ: bộ nhớ flash, cảm biến).
     - **Timer:** PC0-PC7 hỗ trợ các kênh Timer 1 và Timer 2, bao gồm điều khiển PWM và đếm thời gian.
     - **Chuyển đổi tín hiệu ADC:** PC4 (A2) hỗ trợ chức năng ADC để đo tín hiệu analog.

**GPIOD (PD0 - PD7)**
   - **Chức năng chính:**
     - **Giao tiếp UART:** PD0 (UTX), PD1 (URX) hỗ trợ truyền và nhận dữ liệu qua UART.
     - **Giao tiếp I2C:** PD0 (SDA), PD1 (SCL) hỗ trợ truyền dữ liệu với các thiết bị qua giao tiếp I2C.
     - **Reset vi điều khiển:** PD7 (NRST) đóng vai trò là chân reset cho vi điều khiển.
     - **Timer & PWM:** PD0-PD7 hỗ trợ các kênh Timer 1 và Timer 2 cho việc đếm thời gian, điều khiển tín hiệu PWM.
     - **Bộ khuếch đại hoạt động (OP):** PD0 và PD7 hỗ trợ các chức năng của bộ khuếch đại tương tự để xử lý tín hiệu tương tự.

**Tóm tắt:**
- **GPIOA:** Chủ yếu phục vụ dao động, ADC, Timer, và bộ khuếch đại hoạt động.
- **GPIOC:** Hỗ trợ giao tiếp UART, I2C, SPI, Timer và ADC.
- **GPIOD:** Hỗ trợ giao tiếp UART, I2C, Timer, Reset, và bộ khuếch đại hoạt động.

Những nhóm chân này cung cấp các giao diện quan trọng để vi điều khiển giao tiếp với các thiết bị ngoại vi, điều khiển tín hiệu số và tương tự, cũng như quản lý thời gian chính xác cho các ứng dụng điều khiển.


## Datasheet

-   **Datasheet**:  CH32V003RM.PDF-[https://www.wch.cn/downloads/CH32V003RM_PDF.html](https://www.wch.cn/downloads/CH32V003RM_PDF.html)
-   **Reference**: CH32V003DS0.PDF-[https://www.wch.cn/downloads/CH32V003DS0_PDF.html](https://www.wch.cn/downloads/CH32V003DS0_PDF.html)
-   **Core Manual**: QingKeV2_Processor_Manual.PDF-[https://www.wch.cn/downloads/QingKeV2_Processor_Manual_PDF.html](https://www.wch.cn/downloads/QingKeV2_Processor_Manual_PDF.html)

## Môi trường phát triển
### MounRiver Studio IDE
MounRiver Studio là môi trường phát triển chính thức do WCH cung cấp, dựa trên nền tảng Eclipse và được phát triển dưới dạng mã nguồn đóng. Phần mềm này hỗ trợ các hệ điều hành Windows, Linux, và Mac. Bạn có thể tải MounRiver Studio miễn phí tại đây [MounRiver Studio IDE](http://www.mounriver.com/)
![enter image description here](https://img.wch.cn/20230104/cdb2551f-10b4-40df-bb4b-baa81040bbd3.png)

### Open-Source GCC Toolchain

Bạn có thể tải toàn bộ công cụ phát triển (GCC và OpenOCD) cho Linux và Mac từ trang web của MounRiver Studio. Tuy nhiên, các ví dụ được cung cấp ở đây dựa trên dự án [ch32v003fun](https://github.com/cnlohr/ch32v003fun) của CNLohr. Hãy làm theo hướng dẫn trên trang Github để cài đặt bộ công cụ phát triển.

Để cài đặt trình biên dịch GCC trên Linux, bạn có thể sử dụng các lệnh sau:
```
sudo apt install build-essential libnewlib-dev gcc-riscv64-unknown-elf
```
### Hỗ trợ Arduino IDE và PlatformIO 
Hiện có các dự án nhằm làm cho CH32V003 tương thích với Arduino IDE ([arduino_core_ch32](https://github.com/openwch/arduino_core_ch32) and [arduino-wch32v003](https://github.com/AlexanderMandera/arduino-wch32v003)) và PlatformIO ([platform-ch32v](https://github.com/Community-PIO-CH32V/platform-ch32v))



## Nạp code và debug chương trình
### WCH-LinkE

Để có thể nạp code cho **CH32V002** ta dùng mạch nạp  **WCH-LinkE** 

![enter image description here](https://raw.githubusercontent.com/wagiminator/Development-Boards/main/CH32V003F4P6_DevBoard/documentation/CH32V003_wch-linke.jpg)

và phần mềm [WCH-LinkUtility](https://www.wch.cn/downloads/WCH-LinkUtility_ZIP.html) để nạp file `.hex` 


![enter image description here](https://img.wch.cn/20221229/400733f5-6d89-4cab-8e23-a084a1398c4d.png)

Ngoài ra bạn có thể dùng mạch nạp **WCH-LinkE** cho các dòng chip khác của hãng này, bạn có thể tham khảo tại đây : [**WCH-LinkUserManual.PDF**](https://www.wch.cn/downloads/WCH-LinkUserManual_PDF.html)
### Sơ đồ đấu chân
![enter image description here](https://github.com/minhthu0204/ch32v003/blob/main/annh/wch-linke.png?raw=true)

Khi sử dụng chế độ SDI để nạp chương trình và debug , thường cần đến 4 đường tín hiệu: VCC để cấp nguồn, GND để nối đất, SWDIO cho dữ liệu, và SWCLK cho xung nhịp. Tuy nhiên, đối với bo mạchCH32V003F4P6, khi sử dụng WCH-LinkE chỉ cần kết nối duy nhất đường SWDIO đến chân PD1, không cần dùng đến SWCLK, giúp đơn giản hóa giao tiếp thành giao tiếp một dây.

## Tạo chương trình nhấp nháy led cơ bản
### Code chương trình

   - **Tạo Project trong mounRiver**: 
		Vào **File** -> **New**  ->  **MounRiver Project**

![enter image description here](https://github.com/minhthu0204/ch32v003/blob/main/annh/anh1.png?raw=true)
- **Chọn board** :
   Tạo tên thư mục và loại board như hình
   

![enter image description here](https://github.com/minhthu0204/ch32v003/blob/main/annh/anh2.png?raw=true)

- **Coding** :
Mở file `main.c` trong thư mục `User`

![enter image description here](https://github.com/minhthu0204/ch32v003/blob/main/annh/anh5.png?raw=true)

Nhập đoạn code sau vào:

    int main(void)
    {
        GPIO_InitTypeDef gpioInit;
        RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);
        gpioInit.GPIO_Mode = GPIO_Mode_Out_PP;
        gpioInit.GPIO_Pin = GPIO_Pin_0;
        gpioInit.GPIO_Speed = GPIO_Speed_50MHz;
        GPIO_Init(GPIOC, &gpioInit);
        Delay_Init();
        while (1)
        {
        GPIO_SetBits(GPIOC, GPIO_Pin_0);
        Delay_Ms(100);
        GPIO_ResetBits(GPIOC, GPIO_Pin_0);
        Delay_Ms(100);
    
        }
    }
    
- **Build chương trình** :
Làm như hình hoặc nhấn tổ hợp phím `Ctrl +B`

![enter image description here](https://github.com/minhthu0204/ch32v003/blob/main/annh/Anh3.png?raw=true)

File `.hex` được tạo ra trong thư mục `obj`

![enter image description here](https://github.com/minhthu0204/ch32v003/blob/main/annh/anh6.png?raw=true)

- **Nạp Code trực tiếp trong mounRiver** :
Nhấn `F8` hoặc làm như hình

![enter image description here](https://github.com/minhthu0204/ch32v003/blob/main/annh/anh4.png?raw=true)

- **Nạp file `.hex`** :
Mở phần mềm **WCH-LinkUtility** chọn loại board để nạp -> set chế độ truyền dữ liệu cho **WCH-linkRV** -> chọn file `.hex` để nạp  -> nhấn nút như hình hoặc tổ hợp phím `Alt + F4`
![enter image description here](https://github.com/minhthu0204/ch32v003/blob/main/annh/anh7.png?raw=true)
