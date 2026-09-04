# LazySec API
The LazySec API obfuscates Lua source files and returns the obfuscated file as a download.
## Base URL
```text
https://api.lazysec.xyz
```
## Authentication
Every obfuscation request requires a bearer token:
```http
Authorization: Bearer [token]
```
Keep API tokens private. You wont get a reset.
## Health check
Use the health endpoint to confirm that the API is online:
```bash
curl https://api.lazysec.xyz/health
```
Expected response:
```json
{"status":"ok"}
```
## Obfuscate a file
### Request
```http
POST /v1/obfuscate
Content-Type: multipart/form-data
Authorization: Bearer [token]
```
The uploaded file must be sent in a multipart field named `file`.
Accepted file extensions:
- `.lua`
- `.txt`
The default maximum upload size is **2 MB**.
### cURL
```bash
curl -sS -X POST "https://api.lazysec.xyz/v1/obfuscate" \
  -H "Authorization: Bearer [token]" \
  -F "file=@input.lua" \
  -o output.lua
```
On Windows CLI:
```bat
curl -sS -X POST "https://api.lazysec.xyz/v1/obfuscate" ^
  -H "Authorization: Bearer YOUR_API_TOKEN" ^
  -F "file=@input.lua" ^
  -o output.lua
```
## Successful response
The response body is the obfuscated Lua file.
Typical response headers:
```http
HTTP/2 200
Content-Type: application/octet-stream
Content-Disposition: attachment; filename="input_lazysec.lua"
X-Obfuscation-Tier: advertise
```
## Error responses
Errors are returned as JSON with a `detail` field.
### Missing or invalid token — `401`
```json
{"detail":"Invalid or missing API token"}
```
### Unsupported file type — `415`
```json
{"detail":"Only .lua and .txt files are accepted"}
```
### File too large — `413`
```json
{"detail":"File is too large; maximum size is 2 MB"}
```
### Obfuscation failure — `500`
```json
{"detail":"Obfuscation failed"}
```
### Obfuscation timeout — `504`
```json
{"detail":"Obfuscation timed out"}
```
## Minimal integration example
```bash
curl -sS \
  -H "Authorization: Bearer [token]" \
  -F "file=@input.lua" \
  "https://api.lazysec.xyz/v1/obfuscate" \
  -o output.lua
```
