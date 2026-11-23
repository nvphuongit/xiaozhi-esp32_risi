# Hướng dẫn và tính năng Xiaozhi ESP32 Vietnam

## Tổng quan

Xiaozhi ESP32 Vietnam là bản tùy chỉnh của dự án chatbot AI giọng nói Xiaozhi ESP32 dành riêng cho cộng đồng Việt Nam. Dự án kết hợp ESP32, mô hình AI, giọng nói và giao thức MCP để hỗ trợ trò chuyện, điều khiển IoT và phát đa phương tiện.

Dự án được fork từ [xiaozhi-esp32](https://github.com/78/xiaozhi-esp32).

## Tính năng đã phát triển

### Giải trí và đa phương tiện

- Phát nhạc Việt Nam từ máy chủ music có thể cấu hình linh hoạt.
- Có thể thay đổi liên kết máy chủ music mà không cần build lại firmware.
- Phát radio VOV, radio tiếng Anh và các kênh radio khác.
- Hiển thị phổ âm thanh trực quan trên màn hình LCD hoặc OLED.
- Phát video và nhạc từ thẻ nhớ SD trên một số cấu hình màn hình.
- Otto Robot hỗ trợ partition riêng để build firmware điều khiển động cơ servo.

### Cập nhật firmware

- OTA trực tuyến qua mạng: `https://ota-server.xiaozhi-ota.workers.dev/ota/`.
- OTA qua Web Server nội bộ; truy cập theo địa chỉ IP của thiết bị, ví dụ `http://102.168.1.10/ota`.
- Nạp firmware trực tiếp bằng trình duyệt thông qua Web Flasher, không bắt buộc cài đặt công cụ phức tạp.

### Phần cứng được hỗ trợ

- Xingzhi Cube.
- Bo mạch tự lắp theo sơ đồ kết nối có sẵn trong tài liệu và hình ảnh dự án.
- Otto Robot Board.
- Chip ESP32-C3, ESP32-S3 và ESP32-P4.
- Màn hình OLED và LCD.
- Quản lý pin và hiển thị biểu cảm.

## Tính năng cốt lõi

### Kết nối và âm thanh

- Wi-Fi.
- ML307 Cat.1 4G.
- Giao thức WebSocket hoặc MQTT kết hợp UDP.
- Codec âm thanh OPUS.

### AI và xử lý giọng nói

- Wake word offline bằng ESP-SR.
- Nhận dạng giọng nói ASR, mô hình ngôn ngữ LLM và tổng hợp giọng nói TTS theo luồng.
- Nhận dạng loa bằng 3D Speaker.
- Hỗ trợ đa ngôn ngữ: tiếng Trung, tiếng Anh và tiếng Nhật.

### Điều khiển IoT

- MCP phía thiết bị: điều khiển âm lượng, LED, motor, GPIO và các chức năng nội bộ.
- MCP đám mây: điều khiển nhà thông minh, desktop và email.
- Tùy chỉnh wake word, font chữ và biểu cảm.

## Tính năng đang phát triển hoặc dự kiến

- Phát nhạc qua Bluetooth.
- Mã QR thanh toán ngân hàng Việt Nam.
- Cập nhật giá vàng và giá cổ phiếu.
- Tin tức tài chính Việt Nam.
- ANCS Bluetooth trên iPhone để nhận thông báo đường đi và tin nhắn.
- Thông tin cuộc gọi từ điện thoại.
- Web Server cấu hình chân GPIO.
- Điều chỉnh mic gain trực tiếp qua giao diện.
- Hỗ trợ nâng cấp firmware từ phiên bản V1 lên V2.
- Hẹn giờ và quản lý nhiều báo thức.
- Wakeup chủ động theo lịch và thông báo văn bản.
- ModBus RTU và ModBus TCP/IP.
- Một số tính năng nâng cấp màn hình và giao diện đang được bổ sung.

Trạng thái tài liệu:

- Đã phát triển: tính năng đã có trong dự án hoặc firmware.
- Đang phát triển: tính năng đang được code và kiểm thử.
- Kế hoạch: tính năng đã được đề xuất, chưa bắt đầu triển khai.

## Hướng dẫn cho người dùng

1. Truy cập Web Flasher: `https://tienhuyiot.github.io/esp_web_flasher/`.
2. Kết nối ESP32 với máy tính qua USB.
3. Chọn firmware phù hợp rồi nhấn Flash.
4. Bật thiết bị và kết nối vào Wi-Fi do ESP32 phát ra.
5. Cấu hình Wi-Fi gia đình hoặc doanh nghiệp.
6. Nói từ đánh thức, mặc định trong tài liệu là `Sophia`, để bắt đầu hội thoại.

## Yêu cầu cho nhà phát triển

- IDE: VS Code, Cursor hoặc antigravity.
- Plugin ESP-IDF phiên bản 5.4 trở lên.
- Linux được khuyến nghị; Windows cũng được hỗ trợ.
- Tuân thủ Google C++ Style Guide.
- Python 3.9 trở lên.

## Build firmware từ mã nguồn

Clone dự án nếu chưa có mã nguồn:

```bash
git clone https://github.com/TienHuyIoT/xiaozhi-esp32_vietnam.git
cd xiaozhi-esp32_vietnam
```

### ESP-IDF và công cụ `idf.py`

`idf.py` là công cụ dòng lệnh chính của Espressif ESP-IDF để cấu hình, build, nạp và giám sát firmware ESP32:

- `idf.py set-target esp32s3`: chọn chip ESP32-S3.
- `idf.py menuconfig`: mở cấu hình board và tính năng.
- `idf.py fullclean`: xóa dữ liệu build.
- `idf.py build`: build firmware.
- `idf.py flash`: nạp firmware.
- `idf.py monitor`: xem log Serial.
- `idf.py merge-bin`: tạo binary hợp nhất.

### Kích hoạt ESP-IDF trên Windows

Trong môi trường đã kiểm tra, ESP-IDF v5.5.1 được cài tại `C:\Espressif\v5.5.1\esp-idf`. Mở PowerShell và kích hoạt môi trường:

```powershell
$env:PATH="C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python39_64;$env:PATH"
Set-Location C:\Espressif\v5.5.1\esp-idf
& .\export.ps1
```

Nếu ESP-IDF được cài ở thư mục khác, thay đường dẫn tương ứng cho `Set-Location` và script `export.ps1`.

Sau môi trường kích hoạt thành công, vào thư mục dự án:

```powershell
Set-Location F:\MyGitProject\xiaozhi-esp32_vietnam
```

Thông báo thành công sẽ có dòng:

```text
Done! You can now compile ESP-IDF projects.
```

Môi trường ESP-IDF chỉ có hiệu lực trong cửa sổ PowerShell hiện tại. Nếu mở terminal mới, chạy lại `export.ps1`.

### Quy trình build chi tiết

1. Chọn target ESP32-S3:

```bash
idf.py set-target esp32s3
```

2. Mở cấu hình board và tính năng:

```bash
idf.py menuconfig
```

Chọn đúng board phần cứng, lưu bằng `S`, rồi thoát bằng `Q`.

3. Chỉ khi cần dọn build cũ:

```bash
idf.py fullclean
```

Sau `fullclean`, đặt lại target trước khi build:

```bash
idf.py set-target esp32s3
```

4. Build firmware:

```bash
idf.py build
```

Các lệnh phải đứng trên từng dòng riêng. Không ghép thành một dòng như:

```text
idf.py fullcleanidf.py set-target esp32s3idf.py menuconfigidf.py build
```

Nếu máy có ít RAM và compiler báo `cc1plus.exe: out of memory`, build tuần tự:

```powershell
$env:CMAKE_BUILD_PARALLEL_LEVEL="1"
idf.py build
```

### Kết quả build

Sau build thành công, firmware ứng dụng nằm tại:

```text
build\xiaozhi_vn.bin
```

Các file nạp liên quan nằm tại:

```text
build\bootloader\bootloader.bin
build\partition_table\partition-table.bin
build\ota_data_initial.bin
build\generated_assets.bin
```

Trong một build ESP32-S3 mặc định đã kiểm tra:

- Firmware app khoảng `3.26 MB`.
- Partition app nhỏ nhất khoảng `4.13 MB`.
- Dung lượng còn trống khoảng `860 KB`, tương đương `21%`.

Dung lượng thực tế có thể thay đổi khi đổi board hoặc bật thêm tính năng.

### Nạp và giám sát firmware

```bash
# COM_PORT là cổng USB thực tế, ví dụ COM14
idf.py -p COM_PORT flash monitor

# Tạo file binary hợp nhất
idf.py merge-bin
```

Trên Windows có thể xác định cổng COM bằng PowerShell:

```powershell
Get-PnpDevice -PresentOnly | Where-Object {
    $_.FriendlyName -match "USB|COM|Serial"
} | Select-Object FriendlyName,InstanceId
```

Thoát monitor bằng tổ hợp `Ctrl+]`.

Ví dụ nạp thủ công toàn bộ image bằng `esptool.py`:

```powershell
python -m esptool --chip esp32s3 -b 460800 --before default_reset --after hard_reset write_flash --flash_mode dio --flash_size 16MB --flash_freq 80m 0x0 build\bootloader\bootloader.bin 0x8000 build\partition_table\partition-table.bin 0xd000 build\ota_data_initial.bin 0x20000 build\xiaozhi_vn.bin 0x800000 build\generated_assets.bin
```

Giá trị `--flash_size 16MB` phải khớp với bộ nhớ flash thực tế của bo mạch.

### Quy trình build trong VS Code

Khi làm việc với ESP-IDF trong VS Code hoặc Cursor:

- Nên dùng các lệnh ESP-IDF của extension thay vì chạy `idf.py` trực tiếp trong terminal thường.
- Các lệnh phổ biến: Build, Flash, Monitor, Build/Flash/Monitor, Full Clean, Size, Menuconfig và Set Target.
- Luôn kích hoạt đúng môi trường ESP-IDF 5.5 trước khi chẩn đoán bằng terminal.
- Kiểm tra lỗi code, cấu hình, dependency và môi trường theo từng loại thay vì chỉ đọc dòng lỗi cuối.
- Ưu tiên Full Clean chỉ khi cần; với lỗi tệp bị khóa trên Windows, chỉ dọn artifact bị ảnh hưởng rồi build lại.
- Sau thay đổi media, board, codec, màn hình, partition hoặc component manager, phải build lại để xác nhận.
- Sau thay đổi tính năng dùng nhiều bộ nhớ, nên chạy phân tích dung lượng.

### Lỗi thường gặp

#### `idf.py` không được nhận diện

- Chưa kích hoạt môi trường bằng `export.ps1`.
- Đang ở terminal mới chưa nạp ESP-IDF.
- Các lệnh bị ghép trên cùng một dòng.

Kiểm tra công cụ:

```powershell
idf.py --version
```

Nếu không tìm thấy `idf.py`, chạy lại phần kích hoạt ESP-IDF ở trên.

#### `cmake was not found`

Chạy lại `export.ps1` để đưa CMake, Ninja và các công cụ ESP-IDF vào `PATH`. CMake không nên chỉ được cài độc lập nếu chưa kích hoạt bộ công cụ ESP-IDF.

#### Python quá cũ

ESP-IDF v5.5 yêu cầu Python 3.9 trở lên. Đưa Python 3.9 hoặc mới hơn vào đầu `PATH`, sau đó kích hoạt lại môi trường:

```powershell
$env:PATH="C:\Program Files (x86)\Microsoft Visual Studio\Shared\Python39_64;$env:PATH"
python --version
Set-Location C:\Espressif\v5.5.1\esp-idf
& .\export.ps1
```

#### `Could NOT find ZLIB`

Đây có thể là cảnh báo phụ thuộc khi cấu hình. Cần xem các dòng `FAILED`, `CMake Error`, `fatal error` hoặc `ninja failed` ở phần cuối để xác định lỗi thực tế.

#### `cc1plus.exe: out of memory`

Build tuần tự để giảm RAM:

```powershell
$env:CMAKE_BUILD_PARALLEL_LEVEL="1"
idf.py build
```

Nếu vẫn hết RAM, đóng ứng dụng khác hoặc khởi động lại máy trước khi build.

#### Không tìm thấy cổng COM

- Kiểm tra cáp USB hỗ trợ truyền dữ liệu.
- Cài driver USB của chip nạp.
- Kiểm tra lại Device Manager.
- Nhấn BOOT khi bo mạch yêu cầu chế độ nạp.

## Tài liệu kỹ thuật

- Hướng dẫn bo mạch tùy chỉnh: `docs/custom-board.md`
- MCP - hướng dẫn sử dụng: `docs/mcp-usage.md`
- MCP - quy trình tương tác: `docs/mcp-protocol.md`
- MQTT và UDP: `docs/mqtt-udp.md`
- WebSocket: `docs/websocket.md`

## Quy trình đóng góp

1. Fork repository.
2. Tạo branch mới cho tính năng hoặc bản vá.
3. Commit với thông điệp rõ ràng bằng tiếng Việt hoặc tiếng Anh.
4. Push branch lên GitHub.
5. Tạo Pull Request.
6. Tuân thủ Google C++ Style Guide và kiểm thử kỹ trước khi gửi PR.

## Hỗ trợ và cộng đồng

- Video hướng dẫn build: `https://youtu.be/g7Lh-LpxElU`
- Hướng dẫn nạp firmware: `https://ccnphfhqs21z.feishu.cn/wiki/Zpz4wXBtdimBrLk25WdcXzxcnNS`
- Facebook Fanpage: `https://www.facebook.com/XiaozhiAI.IoTVietnam/`
- Facebook Group: `https://www.facebook.com/groups/2655614131443031`
- GitHub Issues: `https://github.com/TienHuyIoT/xiaozhi-esp32_vietnam/issues`

Các nhóm Zalo cũ đã tạm dừng để tổ chức lại cộng đồng. Discord và kênh hỗ trợ mới sẽ được công bố sau.
