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

## Build firmware từ mã nguồn

```bash
git clone https://github.com/TienHuyIoT/xiaozhi-esp32_vietnam.git
cd xiaozhi-esp32_vietnam
idf.py fullclean
idf.py set-target esp32s3
idf.py menuconfig
idf.py build
```

Sau khi build thành công:

```bash
# COM_PORT là cổng USB thực tế, ví dụ COM14
idf.py -p COM_PORT flash monitor

# Tạo file binary hợp nhất
idf.py merge-bin
```

### Quy trình build trong VS Code

Khi làm việc với ESP-IDF trong VS Code hoặc Cursor:

- Nên dùng các lệnh ESP-IDF của extension thay vì chạy `idf.py` trực tiếp trong terminal thường.
- Các lệnh phổ biến: Build, Flash, Monitor, Build/Flash/Monitor, Full Clean, Size, Menuconfig và Set Target.
- Luôn kích hoạt đúng môi trường ESP-IDF 5.5 trước khi chẩn đoán bằng terminal.
- Kiểm tra lỗi code, cấu hình, dependency và môi trường theo từng loại thay vì chỉ đọc dòng lỗi cuối.
- Ưu tiên Full Clean chỉ khi cần; với lỗi tệp bị khóa trên Windows, chỉ dọn artifact bị ảnh hưởng rồi build lại.
- Sau thay đổi media, board, codec, màn hình, partition hoặc component manager, phải build lại để xác nhận.
- Sau thay đổi tính năng dùng nhiều bộ nhớ, nên chạy phân tích dung lượng.

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
