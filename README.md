# Docker Templates for Jupyter Notebooks

This repository offers Docker templates to streamline your initiation into the world of Jupyter Notebooks. These templates are designed to remove the hassle from setting up your Jupyter Notebook environment, letting you dive straight into your projects. [Jupyter Notebooks](https://jupyter.org/) are renowned for their ability to create and share documents that contain live code, equations, visualizations, and narrative text, making them an invaluable tool in data science, machine learning, and more. By utilizing our [Docker](https://www.docker.com/) templates, we can effortlessly set up a Jupyter Notebook environment that fosters efficiency and collaboration.

## Prerequisites

Before you begin exploring the templates, ensure that you have the following software installed on your system:

1. **[Docker](https://docs.docker.com/get-docker/)** : Docker is essential for creating, deploying, and running our templates. It allows you to package your Jupyter Notebook along with all its dependencies in a container, ensuring consistency and ease of deployment.
2. **[CUDA](https://developer.nvidia.com/cuda-toolkit)** (Optional): If you plan to utilize GPU acceleration for your projects, installing CUDA is a prerequisite. It enables your GPU to perform computations, significantly speeding up processes, especially in data science and machine learning tasks.

Please refer to the linked pages for detailed installation guides. Once you have set up the necessary software, you are all set to make the most of our Docker templates for Jupyter Notebooks.

## Selecting the Right Docker Image

Choosing the right Docker image is a crucial step in setting up your project environment efficiently. In this repository, we offer several Dockerfile configurations tailored to different project needs. Here's a guide to help you select the one that suits your project the best:

1. **Dockerfile.cuda** : This configuration provides basic GPU availability without any pre-installed machine learning libraries. It is an excellent choice for projects focusing on data analysis utilizing tools such as scikit-learn.
2. **Dockerfile.cuda.torch** : Building upon the base CUDA image, this configuration additionally includes the Torch library. It is ideal for initiating new machine learning projects or when working with Hugging Face transformers.
3. **Dockerfile.python.base** : A straightforward setup offering a Python-based server, suitable for a wide range of Python projects that do not require specialized libraries or tools.
4. **Dockerfile.python.spark** : This configuration comes with Java 11 and Spark installed, with PySpark pre-configured. It is perfect for projects that require big data processing and analysis capabilities.

#### Updating the Docker-Compose File

Once you have selected the appropriate Docker image, you will need to update the `docker-compose.yml` file in the following manner:

* **Project Setup** : Instead of working directly from the repository, we recommend copying the necessary files to a new project directory. This practice ensures a clean and organized workspace, allowing for more straightforward customization and scaling.
* **Update the Image Name** : Replace the existing image name (`dockerfile`) with the filename of the Docker image you have chosen from the options above.
* **NVIDIA Runtime** : If your project does not require NVIDIA runtime, it is recommended to remove it from the configuration to save resources.

## Managing Your Docker Environment

Managing your Docker environment efficiently is crucial for a smooth and productive workflow. Here, we outline the steps to build, start, stop, and destroy your Docker containers using two strategies: Docker Compose and Docker commands. For GPU-enabled projects, we recommend using Docker Compose for its simplicity and ease of management.

#### Using Docker Compose

Docker Compose facilitates the management of multi-container Docker applications. Here are the basic commands to manage your project:

1. **Build Your Project**:

   ```
   docker compose build
   ```
2. **Start Your Project**:

   ```
   docker compose up
   ```
3. **Stop Your Project**:

   ```
   docker compose down
   ```
4. **Destroy Your Project**:

   ```
   docker compose down --volumes --remove-orphans
   ```

#### Using Docker Commands

If you prefer working directly with Docker commands, here are the basic steps to manage your Docker image individually:

1. **Build Your Image**:

   ```
   docker build -t your_image_name -f your_dockerfile_name .
   ```
2. **Run Your Image**:

   ```
   docker run -d --name your_container_name your_image_name
   ```
3. **Stop Your Container**:

   ```
   docker stop your_container_name
   ```
4. **Remove Your Container**:

   ```
   docker rm your_container_name
   ```

#### Recommendation for GPU-Enabled Projects

For projects that leverage GPU acceleration, we highly recommend using the Docker Compose strategy. It offers a simpler and more streamlined approach to managing GPU resources, making it easier to focus on your project without getting bogged down in configuration details.

#### You can access your Jupyter env from your web browser

`http://localhost:8888/?token=token`

#### If you need further see the host based on your configuration you can use following commands:

To get the image name you want to work with

`docker ps`

Docker compose:

`docker-compose exec your_container_name jupyter notebook list`

Docker:

`docker exec -it your_container_name jupyter notebook list`

## Configure the Nvidia runtime Debian base OS (WSL: Debian, Ubuntu)

These packages need be installed in order to use your local GPU.
`sudo apt update -y && sudo apt install jq nvidia-container-runtime -y`

If you don't have anything custom this will be easiest way.
`sudo mv /etc/docker/daemon.json /etc/docker/daemon.json.bak`

Create a new docker daemon config file.
`sudo vim /etc/docker/daemon.json`
Copy this code into the file (Don't forget to save :)).

```json
{
 "runtimes": {
  "nvidia": {
    "path": "/usr/bin/nvidia-container-runtime",
    "runtimeArgs": []
  }
 }
}
```

Configure the runtime
`sudo nvidia-ctk runtime configure`

Restart docker.
`sudo systemctl restart docker`
or
`sudo service docker restart`

## Setting Up Jupyter Notebook with VSCode

To streamline your data science and machine learning projects, you can set up a Jupyter Notebook environment within VSCode. This setup allows you to access the Jupyter Notebook server running in a Docker container directly from VSCode. Here's how you can do it:

#### Step 1: Install Necessary Extensions

Ensure that you have the [Jupyter extension](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter) installed in your VSCode to facilitate the integration with Jupyter Notebooks.

#### Step 2: Launch VSCode

Open VSCode and create a new Jupyter Notebook by clicking on the new file icon and selecting the Jupyter Notebook option or simply by creating a new file with a `.ipynb` extension.

#### Step 3: Configure Jupyter Server Connection

Select your kernel by clicking on the kernel picker in the top right of the notebook or by invoking the `Notebook: Select Notebook Kernel` command and then, select the option `Connect to Jupyter Server`. A prompt will appear asking for the Jupyter server URI.

#### Step 4: Enter Docker Jupyter URL and Token

At this stage, you would need to enter the URL and token of your running Docker Jupyter server. The URL would typically be `http://127.0.0.1:8888/` (or a different IP and port if you have configured it differently in the Docker container). You will also need to provide the token, which in the case of the provided Docker template is `token`.

Here is how you can construct the full URI with the token:

```
http://127.0.0.1:8888/?token=token
```

Enter this URI in the prompt in VSCode to connect to the Jupyter server running in the Docker container.

#### Step 5: Start Using Jupyter Notebook

Once connected, you can start creating new notebooks, writing code, and running cells directly from VSCode, with computations being performed in the Docker container.

This setup combines the versatility of Jupyter Notebooks with the powerful features of VSCode, offering an efficient and streamlined development environment.
