FROM python:3.12-alpine
RUN adduser -D -H appuser
WORKDIR /app
COPY app.py .
RUN chown -R appuser:appuser /app
USER appuser
EXPOSE 8080
CMD ["python", "app.py"]

