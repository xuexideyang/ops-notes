# Nginx

## 为什么用 Nginx
- 跨平台，配置简单，高并发，内存消耗小。
- 内置健康检查，稳定性高，节省带宽。
- 接收用户请求是异步非阻塞的。
- 反向代理和负载均衡功能强大。

## Nginx 性能为什么比 Apache 高
- Nginx 使用 epoll 模型，Apache 使用 select 模型。
- 类比：epoll 像宿管阿姨知道每个人的房间号，直接带你去；select 像挨个敲门找，效率低。

## 正向代理 vs 反向代理
- 正向代理：代理客户端，隐藏客户端 IP。
- 反向代理：代理服务端，隐藏服务端 IP，可做负载均衡、缓存、访问控制。

## 常见 HTTP 状态码
- 200：成功
- 301：永久重定向
- 302：临时重定向
- 403：禁止访问
- 404：未找到
- 500：服务器内部错误
- 502：网关错误
- 504：网关超时

## master 和 worker 进程
- master：负责读取配置、管理 worker 进程（启动、停止、重载）。
- worker：负责处理实际请求，利用 epoll 实现高并发。

## Nginx 高并发原理
- master 管理多个 worker，worker 是单线程异步非阻塞。
- 采用 epoll 模型，极致利用内存和连接数。

## proxy_pass vs fastcgi_pass
- `proxy_pass`：反向代理 HTTP 服务（如后端 Tomcat、Node.js）。
- `fastcgi_pass`：转发请求到 FastCGI 服务器（如 PHP-FPM）。

## open_file_cache
- 缓存文件句柄和元数据，降低 CPU 和 I/O 开销，提高静态文件服务性能。

## Nginx 限流
- 使用 `limit_req_zone`（漏桶算法）限制请求频率。
- 使用 `burst` 参数允许一定突发流量。

## 惊群问题
- 多个 worker 同时 accept 同一个监听端口，导致多个进程被唤醒但只有一个成功。
- Nginx 使用 `accept_mutex` 互斥锁解决。

## Nginx 负载均衡策略
- 轮询：按顺序轮流分配。
- 加权轮询：设置 weight 值，值越大分配越多。
- ip_hash：根据客户端 IP 计算哈希值，固定分配到同一台服务器。
- least_conn：分配到当前连接数最少的服务器。
- 最快响应（fair，需第三方模块）。

## 虚拟主机类型
- 基于域名、基于端口、基于 IP。

## Nginx 优化
- 调整 worker_processes 为 CPU 核心数，worker_connections 适当增大，开启 epoll。
- 开启 gzip 压缩，减少传输大小。
- 对静态资源设置 expires，减少重复请求。

## Nginx 防盗链
- 在 location 模块中使用 `valid_referers` 定义白名单。
- 使用 `if ($invalid_referer)` 返回 403 或重定向。