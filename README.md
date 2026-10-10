# USBCDCSTM32F407 - Lab02 CE224.R12

Dự án cấu hình và lập trình vi điều khiển **STM32F407VET6** hoạt động như một thiết bị **USB CDC (Virtual COM Port)** truyền nhận dữ liệu 2 chiều qua cổng COM với máy tính.

## Tính năng
- Cấu hình ngoại vi **USB_OTG_FS** hoạt động ở chế độ `Device_Only` (Chân PA11 - DM, PA12 - DP).
- Sử dụng middleware **USB_DEVICE** (lớp Communication Device Class - CDC).
- **Truyền dữ liệu (TX)**: STM32 định kỳ truyền chuỗi `"Hello CE224.R12, this is USB CDC\r\n"` lên máy tính.
- **Nhận dữ liệu (RX)**: Xử lý ngắt nhận dữ liệu `CDC_Receive_FS` từ máy tính và thực hiện Echo phản hồi dữ liệu qua cổng Virtual COM Port.

## Cấu trúc thư mục
- `Core/`: Chứa mã nguồn chính `main.c`, khởi tạo xung nhịp HSE (25MHz) và clock USB 48MHz.
- `USB_DEVICE/`: Cấu hình lớp USB CDC, mã nguồn xử lý ngắt truyền/nhận USB (`usbd_cdc_if.c`).
- `Drivers/`: Thư viện STM32F4xx HAL Driver & CMSIS.
- `Middlewares/`: Thư viện ST STM32 USB Device Library.

## Hướng dẫn nạp và kết nối
1. **Biên dịch dự án**: Sử dụng CMake / Ninja với toolchain `arm-none-eabi-gcc`.
2. **Nạp Firmware**: Sử dụng ST-Link hoặc STM32CubeProgrammer nạp file `.elf`, `.hex` hoặc `.bin` vào kit STM32F407.
3. **Kết nối máy tính**:
   - Cắm cáp USB vào cổng micro-USB (USB OTG FS) của kit STM32.
   - Máy tính sẽ nhận thiết bị dưới dạng `STMicroelectronics Virtual COM Port` hoặc `USB Serial Device`.
   - Có thể gán cổng COM tùy ý trong **Device Manager -> Ports (COM & LPT) -> Properties -> Port Settings -> Advanced**.
4. **Kiểm tra với Terminal**:
   - Mở Hercules / Tera Term / PuTTY / Serial Monitor.
   - Chọn đúng cổng COM, Baudrate: 115200 (hoặc bất kỳ), Data bits: 8, Parity: None, Stop bits: 1.
   - Quan sát dòng chữ hiển thị định kỳ và thử gửi chuỗi để kiểm tra tính năng nhận/echo.
