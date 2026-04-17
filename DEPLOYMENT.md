# Deployment Information

## Public URL
[BẠN HÃY ĐIỀN URL PUBLIC CỦA BẠN VÀO ĐÂY, VD: https://your-agent.railway.app]

## Platform
[BẠN ĐÃ DÙNG NỀN TẢNG NÀO? ĐIỀN VÀO ĐÂY, VD: Railway / Render]

## Test Commands

### Health Check
```bash
curl [URL CỦA BẠN]/health
# Expected: {"status": "ok", ...}
```

### API Test (with authentication)
```bash
curl -X POST [URL CỦA BẠN]/ask \
  -H "X-API-Key: [API KEY CỦA BẠN]" \
  -H "Content-Type: application/json" \
  -d '{"user_id": "test", "question": "Hello"}'
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
