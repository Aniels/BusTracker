# 系統架構說明

## 組成元件

- **FastAPI（Backend）**：處理查詢請求與資料整合
- **PostgreSQL**：儲存靜態資料（站牌、路線、時刻表）
- **MongoDB**：記錄即時位置資料，支援高頻寫入
- **Redis**：即時快取熱門查詢結果
- **Kafka**：負責即時訊息傳遞與訂閱事件串流
- **React + Leaflet**：前端視覺化介面

## 資料流簡述

1. 公車設備定期發送 GPS（模擬）
2. Kafka 接收訊息並推至 MongoDB 儲存與 Redis 快取
3. 使用者查詢 → FastAPI 調用 Redis/Mongo → 回傳結果
4. 若為訂閱事件 → Kafka 廣播事件 → 通知前端（WebSocket）
