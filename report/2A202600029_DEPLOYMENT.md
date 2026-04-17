# Deployment Information

## Public URL

https://day12-vinuni-production.up.railway.app

## Platform

[Railway]

## Test Commands

### Health Check

```bash
curl https://day12-vinuni-production.up.railway.app/health
# Expected: {"status": "ok", ...}
```

### API Test (with authentication)

```bash
curl -X POST https://day12-vinuni-production.up.railway.app/ask \
  -H "X-API-Key: your-secret-key-123" \
  -H "Content-Type: application/json" \
  -d '{"question": "Hello"}'
```

## Environment Variables Set

- PORT
- REDIS_URL
- AGENT_API_KEY
- LOG_LEVEL
- OPENAI_API_KEY (nếu dùng thật)

## Screenshots

[LƯU Ý: HÃY CHỤP 3 ẢNH DƯỚI ĐÂY VÀ LƯU VÀO THƯ MỤC screenshots/ TRONG PROJECT GỐC]

- [Deployment dashboard](screenshots/dashboard.png) - (Ảnh chụp màn hình quản lý trên Railway/Render)
- [Service running](screenshots/running.png) - (Ảnh chụp logs server đang chạy không lỗi)
- [Test results](screenshots/test.png) - (Ảnh chụp màn hình terminal khi test lệnh curl thành công)
