# 使用 linuxserver 提供的 Alpine Selkies 基础镜像
# 该镜像已包含 WebRTC 串流所需的核心 Selkies 框架
FROM ghcr.io/linuxserver/baseimage-selkies:alpine322

# Metadata labels
LABEL org.opencontainers.image.title="CoolRetroTerm Selkies"
LABEL org.opencontainers.image.description="Cool Retro Term in browser via Selkies WebRTC"
LABEL org.opencontainers.image.source="https://github.com/Melieahs/coolretroterm-selkies"
LABEL org.opencontainers.image.vendor="CoolRetroTerm Selkies Project"
LABEL org.opencontainers.image.licenses="GPL-3.0-only"

# set environment variables and install core packages
RUN apk update && \
    # 安装 cool-retro-term 及其依赖（如 Qt5）
    apk add cool-retro-term font-noto-cjk xfce4-terminal && \
    # 清理缓存以减小最终镜像体积
    rm -rf /var/cache/apk/* && \
    echo "✅ Cool Retro Term installation completed"

# --- Selkies 启动配置：使用 Openbox autostart ---

# 1. 确保 Openbox autostart 目录存在 (通常在基础镜像中已存在)
#    我们将在全局 autostart 目录中创建启动脚本。
RUN mkdir -p /etc/xdg/openbox

# 2. 创建 Openbox autostart 脚本
#    Openbox 启动时会自动执行这个脚本中的命令
#    使用 & 让 cool-retro-term 在后台运行，这样 Openbox 才能继续加载
RUN echo '#!/bin/sh' > /etc/xdg/openbox/autostart && \
    echo '/usr/bin/cool-retro-term --fullscreen &' >> /etc/xdg/openbox/autostart && \
    # 如果 cool-retro-term 启动失败，则启动 xfce4-terminal
    echo 'if [ $? -ne 0 ]; then /usr/bin/xfce4-terminal; fi' >> /etc/xdg/openbox/autostart && \
    chmod +x /etc/xdg/openbox/autostart

# 3. 移除之前尝试的 s6 服务 (可选，防止冲突)
RUN rm -rf /etc/services.d/coolretroterm

# --- 环境变量设置 ---
# 设置应用名称
ENV TITLE="Cool Retro Term"
# 设置时区
ENV TZ="Asia/Shanghai"
# 设置中文环境
ENV LC_ALL="zh_CN.UTF-8"
