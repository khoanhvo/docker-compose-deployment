# Ollama Docker Compose

File Docker Compose này cho phép bạn dễ dàng triển khai và chạy Ollama server sử dụng Docker.

## Giới thiệu

Ollama là một công cụ mã nguồn mở cho phép bạn chạy các mô hình ngôn ngữ lớn (LLM) trực tiếp trên máy tính của mình. Docker Compose file này giúp bạn triển khai Ollama server một cách nhanh chóng và dễ dàng.

## Yêu cầu

- Docker và Docker Compose đã được cài đặt trên máy của bạn
- Hệ điều hành: Linux x86_64
- Không yêu cầu GPU

## Cách sử dụng

1. Clone repository này về máy local của bạn.

2. Di chuyển đến thư mục `docker-compose-src/ollama`:

   ```bash
   cd docker-compose-src/ollama
   ```

3. Chạy lệnh sau để khởi động Ollama container:

   ```bash
   docker compose up -d
   ```

4. Ollama API sẽ có thể truy cập tại `http://localhost:11434`.

## Cấu hình

File `docker-compose.yml` bao gồm các cấu hình sau:

- **image**: Sử dụng image chính thức `ollama/ollama:latest`

- **ports**: Ánh xạ cổng 11434 của container tới cổng 11434 trên host, cho phép truy cập API của Ollama

- **volumes**: Mount thư mục `./ollama` trên host tới `/root/.ollama` trong container để lưu trữ models và dữ liệu. Điều này đảm bảo dữ liệu không bị mất khi container bị xóa.

- **restart**: Thiết lập `unless-stopped` để tự động khởi động lại container khi gặp lỗi, trừ khi container bị dừng thủ công

- **healthcheck**: Kiểm tra định kỳ endpoint `http://localhost:11434/api/tags` để đảm bảo service đang hoạt động bình thường

## Sử dụng Ollama

Sau khi container đã chạy, bạn có thể sử dụng Ollama CLI hoặc API:

### Sử dụng CLI

Chạy một mô hình (ví dụ: llama2):

```bash
docker exec -it ollama ollama run llama2
```

### Sử dụng API

Gọi API để lấy danh sách models:

```bash
curl http://localhost:11434/api/tags
```

Chạy inference với API:

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama2",
  "prompt": "Why is the sky blue?"
}'
```

## Kiểm tra trạng thái

Kiểm tra trạng thái của container:

```bash
docker compose ps
```

Xem logs của container:

```bash
docker compose logs -f
```

## Dừng và xóa

Để dừng container:

```bash
docker compose stop
```

Để dừng và xóa container:

```bash
docker compose down
```

**Lưu ý**: Dữ liệu trong thư mục `./ollama` sẽ được giữ lại ngay cả khi container bị xóa.

## Thông tin thêm

- Tài liệu chính thức của Ollama: https://ollama.ai
- GitHub repository: https://github.com/ollama/ollama
- Docker Hub: https://hub.docker.com/r/ollama/ollama
