FROM mcr.microsoft.com/playwright/python:v1.49.0-jammy

# Copy uv from the Astral image
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Set working directory
WORKDIR /code

# Enable bytecode compilation
ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy

# ---- Copy dependency files first ----
COPY pyproject.toml uv.lock /code/

# Install dependencies (no dev)
RUN uv sync --no-dev

# ---- Copy the rest of your app ----
COPY . /code

# Update PATH for virtual environment
ENV PATH="/code/.venv/bin:$PATH"

# Install Playwright and Chromium
RUN playwright install --with-deps chromium

# Expose app port
EXPOSE 3100

# Start the app
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "3100", "--workers", "4"]
