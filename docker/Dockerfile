# syntax=docker/dockerfile:1

FROM python:3.13-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    TF_CPP_MIN_LOG_LEVEL=2

# System dependencies:
# - libgomp1: OpenMP runtime required by XGBoost and scikit-learn
# - fonts-dejavu-core: DejaVu Sans font configured in matplotlib
# - ca-certificates: HTTPS support if dataset fallback download is triggered
RUN apt-get update && apt-get install -y --no-install-recommends \
        libgomp1 \
        fonts-dejavu-core \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace

# Install Python dependencies first for better layer caching
COPY docker/requirements.txt /tmp/requirements.txt
RUN pip install --upgrade pip \
    && pip install -r /tmp/requirements.txt \
    && rm -f /tmp/requirements.txt

# Copy the unified notebook and dataset into the working directory
COPY ["Content/Giai Quyet Bai Toan/ML_BTL_Unified_Womens_Clothing_Reviews.ipynb", "/workspace/"]
COPY ["Content/Giai Quyet Bai Toan/Womens Clothing E-Commerce Reviews.csv", "/workspace/"]

# Run as a non-root user
RUN useradd --create-home --shell /bin/bash mluser \
    && chown -R mluser:mluser /workspace
USER mluser

EXPOSE 8888

# Launch JupyterLab by default (override CMD to execute headless with nbconvert)
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--ServerApp.token=", "--ServerApp.password="]
