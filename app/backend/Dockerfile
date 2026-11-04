FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
# --trusted-host: נדרש בגלל נטפרי (אחרת pip נכשל ב-CERTIFICATE_VERIFY_FAILED)
RUN pip install --no-cache-dir --trusted-host pypi.org --trusted-host files.pythonhosted.org -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
