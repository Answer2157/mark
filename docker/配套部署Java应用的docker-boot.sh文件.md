# 配套部署Java应用的docker-boot.sh文件

~~~sh
#!/bin/bash

# 用户可配置变量（按需修改）
CONTAINER_NAME="gjiao"          # 容器名称
IMAGE_NAME="gj"                 # Docker镜像名称
HOST_PORT=8090                  # 宿主机端口
CONTAINER_PORT=8090             # 容器内部端口
DOCKERFILE_PATH="."             # Dockerfile所在路径

# 依赖的环境变量（需在运行脚本前设置）
JASYPT_ENV="JASYPT_PASSWORD=${JASYPT_PASSWORD}"  # 加密密码

# 检查必要环境变量
check_env() {
  if [ -z "${JASYPT_PASSWORD}" ]; then
    echo "错误：必须设置 JASYPT_PASSWORD 环境变量"
    exit 1
  fi
}

# 构建 Docker 镜像
build_image() {
  echo "正在构建新镜像..."
  docker rmi -f "${IMAGE_NAME}" 2>/dev/null || true
  if ! docker build -t "${IMAGE_NAME}" "${DOCKERFILE_PATH}"; then
    echo "镜像构建失败！"
    exit 1
  fi
}

# 启动容器
start() {
  check_env
  
  # 强制清理旧容器（无论是否正在运行）
  if docker ps -a --format '{{.Names}}' | grep -q "^${CONTAINER_NAME}$"; then
    echo "发现旧容器，正在清理..."
    docker rm -f "${CONTAINER_NAME}" >/dev/null
  fi

  build_image

  # 启动新容器
  docker run -d \
    --name "${CONTAINER_NAME}" \
    -e "${JASYPT_ENV}" \
    -p "${HOST_PORT}:${CONTAINER_PORT}" \
    "${IMAGE_NAME}"

  echo "容器 ${CONTAINER_NAME} 已启动，映射端口：${HOST_PORT}->${CONTAINER_PORT}"
}

# 停止容器
stop() {
  if docker ps --format '{{.Names}}' | grep -q "^${CONTAINER_NAME}$"; then
    docker stop "${CONTAINER_NAME}" >/dev/null
    docker rm "${CONTAINER_NAME}" >/dev/null
    echo "容器 ${CONTAINER_NAME} 已停止并移除"
  else
    echo "容器 ${CONTAINER_NAME} 未在运行"
  fi
}

# 重启容器
restart() {
  stop
  start
}

# 查看日志
logs() {
  docker logs -f --tail 100 "${CONTAINER_NAME}"
}

# 帮助信息
usage() {
  echo "使用方式：$0 {start|stop|restart|logs}"
  exit 1
}

# 主逻辑
case "$1" in
  start)
    start
    ;;
  stop)
    stop
    ;;
  restart)
    restart
    ;;
  logs)
    logs
    ;;
  *)
    usage
    ;;
esac

exit 0
~~~



## 然后使用下面的命令处理换行符问题

~~~
sed -i 's/\r$//' docker-boot.sh
~~~



