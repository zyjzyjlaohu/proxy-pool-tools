FROM python:3.8-alpine

WORKDIR /app

# 安装依赖包
RUN apk add --no-cache gcc musl-dev python3-dev libffi-dev openssl-dev

# 复制项目文件
COPY requirements.txt .

# 安装Python依赖
RUN pip install --no-cache-dir -r requirements.txt

# 复制项目代码
COPY . .

# 暴露端口
EXPOSE 5555

# 启动命令
CMD ["python", "run.py"]
