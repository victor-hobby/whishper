FROM python:3.12-slim

WORKDIR /app

COPY . .

RUN dpkg -i cudnn-local-repo-debian11-8.9.7.29_1.0-1_amd64.deb && \
    cp /var/cudnn-local-repo-*/cudnn-local-*-keyring.gpg /usr/share/keyrings/ && \
    apt-get update && \
    apt-cache policy libcudnn8 && \
    apt-get install libcudnn8=8.9.7.29-1+cuda12.2 && \
    apt-get install libcudnn8-dev=8.9.7.29-1+cuda12.2 && \
    apt-get install libcudnn8-samples=8.9.7.29-1+cuda12.2

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get -qq update && \
    apt-get -qq install ffmpeg curl git && \
    rm -rf /var/lib/apt/lists/* &&\
    pip install --upgrade pip && \
    pip install -r requirements.txt &&\
    pip install python-multipart && \
    rm -rf /root/.cache/pip/ #&& \
    #export LD_LIBRARY_PATH=/usr/local/lib/python3.12/site-packages/nvidia/cublas/lib/:/usr/local/lib/python3.12/site-packages/nvidia/cudnn/lib/

HEALTHCHECK --interval=30s --timeout=10s --start-period=15s \
    CMD ["python3", "healthcheck.py"]

EXPOSE 8000
CMD ["python3", "main.py"]
