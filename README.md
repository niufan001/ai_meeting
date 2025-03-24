# ai_meeting
smart_meeting
# 智能会议分析系统 API 文档

## 概述
本文档遵循 OpenAPI 3.0 规范，包含系统所有接口的详细说明。

## 认证方式
```http
Authorization: Bearer {API_KEY}
```

---

## 语音处理模块

### 长语音识别接口
```http
POST /lfasr
```
**请求格式**：multipart/form-data  
**参数**：
- `file`: 语音文件（wav/pcm）

**响应示例**：
```json
{
  "status": "success",
  "result": "识别文本内容..."
}
```

**错误码**：
- 400：文件格式不支持
- 500：服务器内部错误

### 实时语音识别接口
```http
POST /rtasr
```
**参数**：
- `file`: 实时语音文件（wav）

**响应结构**同长语音接口

---

## 文件管理模块

### 更新文件备注
```http
PUT /update_remark
```
**请求体**：
```json
{
  "filename": "recording_20240520143045.wav",
  "remark": "重要会议记录"
}
```
**响应示例**：
```json
{
  "status": "success"
}
```
**错误码**：
- 404: 文件不存在
- 500: 数据库操作失败

### 删除音频文件
```http
DELETE /delete_audio
```
**查询参数**：
- `filename`: 要删除的文件名

**响应示例**：
```json
{
  "status": "success"
}
```
**错误码**：
- 500: 删除操作失败

### 播放音频文件
```http
GET /play_audio
```
**查询参数**：
- `filename`: 要播放的文件名

**响应**：
`audio/wav` 音频流

**错误码**：
- 404: 文件不存在

### 获取音频列表
```http
GET /list_audio_files
```

**响应示例**：
```json
{
  "files": [
    {
      "name": "recording_20240520143045.wav",
      "size": 102400,
      "ctime": "2024-05-20T14:30:45",
      "remark": "季度规划会议"
    }
  ]
}
```

### 上传音频文件
```http
POST /upload_audio
```
**响应示例**：
```json
{
  "status": "success",
  "filename": "recording_20240520143045.wav"
}
```

### 删除音频文件
```http
DELETE /delete_audio
```
**查询参数**：
- `filename`: 要删除的文件名

---

## 会议记录管理

### 获取分析记录
```http
GET /list_analysis_records
```
**响应示例**：
```json
{
  "records": [
    {
      "id": 1,
      "session": "meeting_123",
      "file_path": "/output/md/analysis_20240520143045.md",
      "created_time": "2024-05-20 14:30:45"
    }
  ]
}
```

### 文本文件分析
```http
POST /upload
```
**请求格式**：multipart/form-data  
**参数**：
- `file`: 文本文件（txt/md）

**响应结构**：
```json
{
  "response": "生成的分析内容...",
  "

**调用示例**：
```bash
curl -X POST \
  -H "Authorization: Bearer {API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"message":"请总结今日会议要点"}' \
  http://localhost:8000/chat
```
```http
POST /chat
```
**请求体**：
```json
{
  "message": "分析会议重点",
  "session_id": "meeting_123"
}
```

**响应结构**：
```json
{
  "response": "会议总结内容...",
  "session_id": "meeting_123"
}
```

完整文档包含 12 个接口说明，涵盖文件管理、语音处理、对话分析等功能模块，每个接口均包含请求示例、响应结构和错误代码说明。
