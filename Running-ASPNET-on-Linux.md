# Running ASP.NET on Linux
=====================================

## Summary
---------------

Running ASP.NET on Linux is a viable option for developers who want to take advantage of the .NET ecosystem while using a Linux-based operating system. This document provides a comprehensive guide on installing and deploying ASP.NET applications on Linux, including detailed instructions, example GitHub actions, and additional resources.

## Installing ASP.NET on Linux
-------------------------------

### Prerequisites

Before you can install ASP.NET on Linux, you need to have the following prerequisites:

*   A Linux-based operating system (e.g., Ubuntu, CentOS)
*   The .NET SDK installed (see below for installation instructions)

### Installing the .NET SDK

To install the .NET SDK on Linux, follow these steps:

1.  Open a terminal and update your package index: `sudo apt-get update` (for Ubuntu-based systems) or `sudo yum update` (for CentOS)
2.  Install the required packages:
    *   On Ubuntu: `sudo apt-get install -y dotnet-sdk-6.0`
    *   On CentOS: `sudo yum install -y dotnet-sdk-6.0`
3.  Verify that the .NET SDK has been installed correctly by running the following command in your terminal: `dotnet --version`

### Installing ASP.NET Core on Linux

Once you have the .NET SDK installed, you can proceed to install ASP.NET Core:

1.  Install the required packages:
    *   On Ubuntu: `sudo apt-get install -y dotnet-aspnetcore3.1`
    *   On CentOS: `sudo yum install -y dotnet-aspnetcore3.1`
2.  Verify that ASP.NET Core has been installed correctly by running the following command in your terminal: `dotnet --list-runtimes`

## Deploying ASP.NET Apps on Linux
-------------------------------------

### Introduction to Deployment

Deploying an ASP.NET application on Linux involves several steps:

*   Creating a new .NET Core project using the `dotnet` CLI
*   Configuring the project for deployment
*   Building and publishing the application
*   Deploying the published application to a Linux server or cloud platform

### GitHub Actions for Deployment

Here's an example GitHub action that demonstrates how to deploy an ASP.NET application on Linux:

```yml
name: CI/CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Setup .NET Core SDK
        uses: actions/setup-dotnet@v1.6.0
        with:
          dotnet-version: '6.0'

      - name: Build and publish app
        run: |
          dotnet build -c Release
          dotnet publish -c Release -o /app

      - name: Deploy to Linux server via SSH
        uses: ssh-deploy@v1
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          password: ${{ secrets.PASSWORD }}
          path: /var/www/
```

### Deploying ASP.NET Apps on a Linux Server or Cloud Platform

Once you have the published application, you can deploy it to a Linux server or cloud platform. Here's an example of deploying an ASP.NET app to a Linux-based Docker container:

```dockerfile
FROM mcr.microsoft.com/dotnet/core/sdk:6.0 AS build-env
WORKDIR /app
COPY . .
RUN dotnet build -c Release -o /app

FROM mcr.microsoft.com/dotnet/core/sdk:6.0 AS runtime
WORKDIR /app
COPY --from=build-env /app/ /app/
ENV ASPNETCORE_ENVIRONMENT="Production"
CMD ["dotnet", "run"]
```

## Troubleshooting and Advanced Topics
---------------------------------------------

### Common Issues

Here are some common issues you may encounter when running or deploying an ASP.NET application on Linux:

*   Missing dependencies or SDK versions
*   Incorrect .NET Core runtime version
*   Incompatible build environment settings

### Optimizing Performance

To optimize performance for your ASP.NET application on Linux, consider the following tips:

*   Use a fast build tool like `dotnet build`
*   Profile and optimize database queries
*   Utilize caching mechanisms (e.g., Redis)

## Additional Resources
-------------------------

For more information about running and deploying ASP.NET applications on Linux, refer to the following resources:

*   [Microsoft documentation](https://learn.microsoft.com/en-us/dotnet/core/tutorials/linux-prerequisites)
*   [GitHub Actions documentation](https://docs.github.com/en/actions)
*   [.NET Core documentation](https://dotnet.microsoft.com/en-us/docs/core)

Citing Sources
--------------

This document has been written based on the following sources:

*   Microsoft Documentation (https://learn.microsoft.com/)
*   GitHub Actions Documentation (https://docs.github.com/en/actions)
*   .NET Core Documentation (https://dotnet.microsoft.com/en-us/docs/core)

Please note that this document is for informational purposes only, and you should consult the official documentation for specific instructions and troubleshooting steps.