# 🗺️ Milestone Plan – BusTracker

> **版本 0.0.1** 最後更新：2025‑04‑19

本文件列出 BusTracker 專案 12 週開發週期的里程碑與任務拆解，供專案管理、進度追蹤與驗收之用。

---

## 里程碑總覽

| 代號 | 里程碑名稱 | 主要交付物 | 週期 | 優先度 |
|------|------------|------------|------|--------|
| **M0** | 項目啟動 & 需求鎖定 | SRS v1.0、專案章程 | W 1 | ★★★★★ |
| **M1** | 基礎基礎設施 & CI/CD | Docker Compose v1、CI pipeline | W 1‑2 | ★★★★★ |
| **M2** | 後端核心 API 與資料管線 | FastAPI skeleton、ERD、定位蒐集 Job | W 3‑4 | ★★★★☆ |
| **M3** | 前端 MVP & 地圖顯示 | React + Leaflet MVP | W 5‑6 | ★★★★☆ |
| **M4** | 即時推送 & 訂閱 | WebSocket/SSE、Redis 快取 | W 7‑8 | ★★★★☆ |
| **M5** | 測試、效能與監控 | 70%+ test cov.、Grafana Dashboard | W 9‑10 | ★★★☆☆ |
| **M6** | 文件、示範與首版發佈 | README v1.0、OpenAPI Docs、v1 Release | W 11‑12 | ★★★☆☆ |

---

## M0 – 項目啟動 & 需求鎖定

| # | 任務 | 產出 | 角色 | 驗收標準 |
|---|------|------|------|-----------|
| 0.1 | 撰寫 SRS | SRS v0.9 | PM / BA | 含功能、非功能需求與優先度 |
| 0.2 | 里程碑排程 | GitHub Projects | PM | M0‑M6 卡片皆建立 |

---

## M1 – 基礎基礎設施 & CI/CD

| # | 任務 | 產出 | 驗收標準 |
|---|------|------|-----------|
| 1.1 | 建立 Repo & Branch Flow | `main / develop` | 條款寫入 CONTRIBUTING |
| 1.2 | 容器化基礎服務 | `docker-compose.yml` | `docker compose up -d` 所有容器健康 |
| 1.3 | GitHub Actions CI | `.github/workflows/ci.yml` | PR 必須通過 lint+test 才能 merge |
| 1.4 | Pre‑commit Hooks | Black / Ruff / ESLint | Push 失敗即阻擋格式錯誤 |

---

## M2 – 後端核心 API 與資料管線

| # | 任務 | 產出 | 驗收標準 |
|---|------|------|-----------|
| 2.1 | FastAPI skeleton | `/backend/app` | 服務可啟動，`/healthz` 回 200 |
| 2.2 | DB ERD 設計 | `schema.png` | 與 SRS 一致，經 Tech Review |
| 2.3 | 定位蒐集 Job | `collector.py` | 每 10 s 寫入 Kafka topic |
| 2.4 | REST API | `GET /routes`, `GET /vehicles` | Swagger UI 可測 |
| 2.5 | Alembic Migrations | `alembic/` | `alembic upgrade head` 成功 |

---

## M3 – 前端 MVP & 地圖顯示

| # | 任務 | 產出 | 驗收標準 |
|---|------|------|-----------|
| 3.1 | React 基底 | `/frontend` | `npm start` 可啟動 Hot reload |
| 3.2 | 路線搜尋 UI | `RouteSearch.tsx` | 300 ms 內完成查詢建議 |
| 3.3 | 地圖 & Marker | `MapView.tsx` | 顯示車輛位置，更新間隔 5 s |
| 3.4 | Axios SDK 封裝 | `/api/*` | 共用型態定義，零 magic 字串 |
| 3.5 | Dev Nginx | `nginx.conf` | FE/BE 代理無 CORS 問題 |

---

## M4 – 即時推送 & 訂閱

| # | 任務 | 產出 | 驗收標準 |
|---|------|------|-----------|
| 4.1 | WebSocket Gateway | `ws.py` | 連線延遲 < 1 s |
| 4.2 | Redis Pub/Sub | `cache.py` | Pub/Sub 每秒 > 100 msg |
| 4.3 | ETA 演算法 | `eta.py` | 誤差 ±30 秒內 (都市路線) |
| 4.4 | 前端 Socket Hook | `useBusSocket.ts` | 雙頁籤同步更新 |
| 4.5 | 訂閱 API | `POST /subscriptions` | 可建立 / 刪除訂閱 |

---

## M5 – 測試、效能與監控

| # | 任務 | 產出 | 驗收標準 |
|---|------|------|-----------|
| 5.1 | pytest Coverage | `htmlcov/` | Coverage ≥ 70% |
| 5.2 | k6 壓測 | `reports/` | 200 req/s、P99 < 300 ms |
| 5.3 | Prometheus & Grafana | `monitoring.md` | Dashboard 顯示 QPS/延遲 |
| 5.4 | Playwright E2E | `/e2e/` | 核心流程通過率 100% |

---

## M6 – 文件、示範與首版發佈

| # | 任務 | 產出 | 驗收標準 |
|---|------|------|-----------|
| 6.1 | 完整文件集 | `README`、`CONTRIBUTING` | 內容符合 v1 功能 |
| 6.2 | OpenAPI → Redoc | `/docs/api` | 靜態頁可瀏覽 |
| 6.3 | Demo Video | `demo.mp4` | 3 分鐘以內、字幕完整 |
| 6.4 | Release v1.0 | Git Tag / Note | 安裝腳本 + 變更紀錄 |
| 6.5 | 雲端部署腳本 | `deploy/` | 新 VM 10 分鐘內可用 |

---

## 風險與緩解

| 風險 | 影響 | 機率 | 緩解措施 |
|------|------|------|-----------|
| 第三方資料源變動 | 定位失準 | 中 | 加入 Adapter Pattern、監控 API 變動 |
| 團隊成員流動 | 進度延遲 | 低–中 | 文件完善、Code Review 交叉 |
| 高併發壓力 | 性能瓶頸 | 中 | 早期壓測、水平擴充預留 |

---

> 📅 **每週流程**：Sprint Planning → Daily Stand‑up → Sprint Review & Retro。
>
> **備註**：里程碑時程依實際 Velocity 作彈性調整。

