Here is a Python code snippet that automates the process you described using the `pygithub` library for GitHub API interaction and the `docker` library for Docker manipulation:
```python

import os
import subprocess
import requests

def main():
    # Get GitHub repository details from user
    github_username = input("Enter your GitHub username: ")
    github_repo_name = input("Enter your GitHub repository name: ")
    github_token = input("Enter your GitHub token: ")

    # Get Docker Hub details from user
    docker_hub_username = input("Enter your Docker Hub username: ")
    docker_hub_password = input("Enter your Docker Hub password: ")

    # Get port number from user
    port_number = input("Enter the port number your container will listen on: ")

    # Clone the GitHub repository
    print("Cloning GitHub repository...")
    subprocess.run(["git", "clone", f"https://{github_token}@github.com/{github_username}/{github_repo_name}.git"])

    # Change into the cloned repository directory
    os.chdir(github_repo_name)

    # Build the Docker image
    print("Building Docker image...")
    subprocess.run(["docker", "build", "-t", f"{docker_hub_username}/{github_repo_name}", "."])

    # Login to Docker Hub
    print("Logging in to Docker Hub...")
    subprocess.run(["docker", "login", "-u", docker_hub_username, "-p", docker_hub_password])

    # Push the Docker image to Docker Hub
    print("Pushing Docker image to Docker Hub...")
    subprocess.run(["docker", "push", f"{docker_hub_username}/{github_repo_name}"])

    # Run the Docker container
    print("Running Docker container...")
    container_id = subprocess.run(["docker", "run", "-d", "-p", f"{port_number}:80", f"{docker_hub_username}/{github_repo_name}"], capture_output=True).stdout.decode("utf-8").strip()

    # Get the container IP address
    container_ip = subprocess.run(["docker", "inspect", "-f", "{{.NetworkSettings.IPAddress}}", container_id], capture_output=True).stdout.decode("utf-8").strip()

    # Print the URL to access the container
    print(f"Container is running at http://{container_ip}:{port_number}")

if __name__ == "__main__":
    main()
```
Please ensure you have installed the `pygithub` and `docker` libraries before running this code.
