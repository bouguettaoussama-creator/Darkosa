FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Hugging Face Spaces (Docker SDK) تتوقع أن السيرفر يخدم على المنفذ 7860
EXPOSE 7860

# مستخدم غير root (أفضل ممارسة موصى بها من Hugging Face)
RUN useradd -m -u 1000 user
USER user
ENV HOME=/home/user
ENV PATH=/home/user/.local/bin:$PATH

CMD ["python", "app.py"]
