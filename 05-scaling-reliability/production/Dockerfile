# Stage 1: Build dependencies
FROM python:3.11-slim as builder

WORKDIR /build

# Cài đặt công cụ build nếu cần (ở đây mock_llm nhẹ nên không cần nhiều)
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc \
    libc6-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements và cài đặt
# Vì không có file requirements.txt riêng, ta cài trực tiếp các thư viện cần thiết
RUN pip install --no-cache-dir --prefix=/install \
    fastapi==0.115.0 \
    uvicorn[standard]==0.30.0 \
    redis==5.0.8 \
    pydantic==2.9.0 \
    psutil==6.0.0

# Stage 2: Final image
FROM python:3.11-slim

WORKDIR /app

# Copy dependencies từ builder
COPY --from=builder /install /usr/local

# Copy code của agent (bao gồm cả thư mục utils)
COPY . .

# Biến môi trường
ENV PYTHONUNBUFFERED=1
ENV ENVIRONMENT=production

# Cổng mặc định
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD python -c "import urllib.request, os; urllib.request.urlopen('http://localhost:' + os.environ.get('PORT', '8000') + '/health')" || exit 1

# Lệnh khởi chạy
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
