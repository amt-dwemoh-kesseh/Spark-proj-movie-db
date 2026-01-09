# Docker Setup Guide for TMDB PySpark Analysis

This guide will help you run the TMDB Movie Analysis project using Docker, avoiding all Windows PySpark setup issues.

## Prerequisites

- **Docker Desktop**: Download and install from [docker.com](https://www.docker.com/products/docker-desktop)
- Ensure Docker Desktop is running (check system tray icon)

## Quick Start

### 1. Start the Project

Open a terminal in the project directory and run:

```bash
docker-compose up
```

This will:

- Pull the `jupyter/pyspark-notebook` image (first time only, ~1-2 GB)
- Install required dependencies (`python-dotenv`)
- Start Jupyter Notebook with PySpark

### 2. Access Jupyter Notebook

Once the container is running, open your browser and navigate to:

```
http://localhost:8888
```

**Note**: Authentication is disabled for easier local development. If you want to enable it, modify the `docker-compose.yml` file.

### 3. Open Your Notebook

In the Jupyter interface:

- Navigate to the `work` folder
- Open `TMDB_Spark_Analysis.ipynb`
- Run your PySpark analysis!

### 4. Stop the Project

When you're done, press `Ctrl+C` in the terminal, or run:

```bash
docker-compose down
```

## Project Structure

```
de-1/
├── docker-compose.yml          # Docker configuration
├── DOCKER_SETUP.md            # This file
├── TMDB_Spark_Analysis.ipynb  # Main analysis notebook
├── requirements.txt           # Python dependencies
├── .env                       # Environment variables (not tracked)
└── .gitignore                 # Git ignore rules
```

## Troubleshooting

### Port 8888 Already in Use

If you get a port conflict error, edit `docker-compose.yml` and change:

```yaml
ports:
  - "8889:8888" # Changed from 8888:8888
```

Then access Jupyter at `http://localhost:8889`

### Permission Issues (Windows)

Ensure Docker Desktop has access to your drive:

1. Open Docker Desktop
2. Go to Settings → Resources → File Sharing
3. Add your project directory

### Container Won't Start

Check Docker logs:

```bash
docker logs tmdb-pyspark-analysis
```

### Need to Install Additional Packages

Open a terminal in Jupyter (New → Terminal) and run:

```bash
pip install package-name
```

Or add the package to the `command` section in `docker-compose.yml`.

## Advanced Usage

### Running in Detached Mode

To run the container in the background:

```bash
docker-compose up -d
```

View logs:

```bash
docker-compose logs -f
```

### Accessing the Container Shell

```bash
docker exec -it tmdb-pyspark-analysis bash
```

### Rebuilding After Changes

If you modify `docker-compose.yml`:

```bash
docker-compose up --build
```

### Using with .env File

The project supports `.env` files for configuration. Your `.env` file will be automatically mounted and accessible via `python-dotenv`.

Example `.env`:

```
API_KEY=your_api_key_here
DATA_PATH=/home/jovyan/work/data
```

## What's Included

The `jupyter/pyspark-notebook` image includes:

- **Apache Spark** (latest stable version)
- **PySpark** with Python 3
- **Jupyter Notebook** and **JupyterLab**
- **Common data science libraries**: pandas, numpy, matplotlib, scipy
- **Java** (required for Spark)

## Why Docker?

Running PySpark on Windows requires:

- Java installation and `JAVA_HOME` configuration
- Hadoop binaries (`winutils.exe`)
- Complex environment variable setup
- Compatibility issues with Windows security manager

Docker eliminates all these issues by providing a pre-configured Linux environment with everything ready to go.

## Support

If you encounter issues:

1. Ensure Docker Desktop is running
2. Check the troubleshooting section above
3. View container logs: `docker-compose logs`
4. Restart Docker Desktop and try again

---

**Happy analyzing!**
