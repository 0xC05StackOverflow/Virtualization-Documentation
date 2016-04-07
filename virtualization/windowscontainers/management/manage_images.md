---
author: neilpeterson
---

# Container images

**This is preliminary content and subject to change.** 

Container images are used to deploy containers. These images can include an operating system, applications, and all application dependencies. For instance, you may develop a container image that has been pre-configured with Nano Server, IIS, and an application running in IIS. This container image can then be stored in a container registry for later use, deployed on any Windows Container host (on-prem, cloud, or even to a container service), and also used as the base for a new container image.

There are two types of container images:

- **Base OS Images** – these are provided by Microsoft and include the core OS components. 
- **Container Images** – a custom container image that is derived from a Base OS image.

## Base OS images

### Install image

Container OS images can be found and installed using the ContainerProvider PowerShell module. Before using this module, it will need to be installed. The following command can be used to install the module.

```none
Install-PackageProvider ContainerProvider -Force
```

Once installed, a list of Base OS images can be returned using `Find-ContainerImage`.

```none
Find-ContainerImage

Name                 Version                 Description
----                 -------                 -----------
NanoServer           10.0.10586.0            Container OS Image of Windows Server 2016 Techn...
WindowsServerCore    10.0.10586.0            Container OS Image of Windows Server 2016 Techn...
```

To download and install the Nano Server base OS image, run the following. The `-version` parameter is optional. Without a base OS image version specified, the latest version will be installed.

```none
Install-ContainerImage -Name NanoServer -Version 10.0.10586.0
```

Likewise, this command will download and install the Windows Server Core base OS image. The `-version` parameter is optional. Without a base OS image version specified, the latest version will be installed.

```none
Install-ContainerImage -Name WindowsServerCore -Version 10.0.10586.0
```

Verify that the images have been installed using the `docker images` command.

```none
docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nanoserver          10.0.14304.1003     40356b90dc80        2 weeks ago         793.3 MB
windowsservercore   10.0.14304.1003     7837d9445187        2 weeks ago         9.176 GB
```  

> If the Base OS image is downloaded, but is not show when running `docker images`, restart the Docker service using the services control panel applet or the command 'sc docker stop' and then 'sc docker start'

### Offline installation

Base OS images can also be installed without an internet connection. To do so, download the image on a computer with an internet connection, copy it to the target system, and then imported using the `Install-ContainerOSImages` command.

Before downloading the Base OS image, prepare the system with the container image provider by running the following command.

```none
Install-PackageProvider ContainerProvider -Force
```

Return a list of images from PowerShell OneGet package manager:

```none
Find-ContainerImage
```

Output:

```
Name                 Version                 Description
----                 -------                 -----------
NanoServer           10.0.10586.0            Container OS Image of Windows Server 2016 Techn...
WindowsServerCore    10.0.10586.0            Container OS Image of Windows Server 2016 Techn...
```

To download an image, use the `Save-ContainerImage` command.

```none
Save-ContainerImage -Name NanoServer -Destination c:\container-image\NanoServer.wim
```

The downloaded container image can now be copied to a different container host, and installed using the `Install-ContainerOSImage` command.

```none
Install-ContainerOSImage -WimPath C:\container-image\NanoServer.wim -Force
```

### Tag images

When referencing a container image by name, the Docker engine will search for the latest version of the image. If the latest version cannot be determined, the following error will be thrown.

```none
docker run -it windowsservercore cmd

Unable to find image 'windowsservercore:latest' locally
Pulling repository docker.io/library/windowsservercore
C:\Windows\system32\docker.exe: Error: image library/windowsservercore not found.
```

After installing the Windows Server Core or Nano Server Base OS images, these will need to be tagged with a version of ‘latest’. To do so, use the `docker tag` command. 

For more information on `docker tag` see [Tag, push, and pull you images on docker.com](https://docs.docker.com/mac/step_six/). 

```none
docker tag <image id> windowsservercore:latest
```

When tagged, the output of `docker images` will show two versions of the same image, one with a tag of the image version, and a second with a tag of 'latest'. The image can now be referenced by name.

```none
docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nanoserver          10.0.14289.1000     df03a4b28c50        2 days ago          783.2 MB
windowsservercore   10.0.14289.1000     290ab6758cec        2 days ago          9.148 GB
windowsservercore   latest              290ab6758cec        2 days ago          9.148 GB
```

### Uninstall OS image

Base OS images can be uninstalled using the `Uninstall-ContainerOSImage` command. The following example will uninstall the NanoServer base OS image.

```none
Uninstall-ContainerOSImage -FullName CN=Microsoft_NanoServer_10.0.14304.1003
```

## Container images

### List Images

```none
docker images

REPOSITORY             TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
windowsservercoreiis   latest              ca40b33453f8        About a minute ago   44.88 MB
windowsservercore      10.0.10586.0        6801d964fda5        2 weeks ago          0 B
nanoserver             10.0.10586.0        8572198a60f1        2 weeks ago          0 B
```

### Create new image

A new container image can be created from any existing container. To do so, use the `docker commit` command. The following example creates a new container image with the name ‘windowsservercoreiis’.

```none
docker commit 475059caef8f windowsservercoreiis
```

### Remove image

Container images cannot be removed if any container, even in a stopped state, has a dependency on the image.

When removing an image with docker, the images can be referenced by image name or id.

```none
docker rmi windowsservercoreiis
```

### Image dependency

To see image dependencies with Docker, the `docker history` command can be used.

```none
docker history windowsservercoreiis

IMAGE               CREATED             CREATED BY          SIZE                COMMENT
2236b49aaaef        3 minutes ago       cmd                 171.2 MB
6801d964fda5        2 weeks ago                             0 B
```

### Docker Hub

The Docker Hub registry contains pre-built images which can be downloaded onto a container host. Once these images have been downloaded, they can be used as the base for Windows Container Applications.

To see a list of images available from Docker Hub use the `docker search` command. Note – the Windows Serve Core or Nano Server base OS images will need to be installed before pulling these dependent container images from Docker Hub.

> The images that start with "nano-" have a dependency on the Nano Server Base OS Image.

```none
docker search *

NAME                    DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
microsoft/aspnet        ASP.NET 5 framework installed in a Windows...   1         [OK]       [OK]
microsoft/django        Django installed in a Windows Server Core ...   1                    [OK]
microsoft/dotnet35      .NET 3.5 Runtime installed in a Windows Se...   1         [OK]       [OK]
microsoft/golang        Go Programming Language installed in a Win...   1                    [OK]
microsoft/httpd         Apache httpd installed in a Windows Server...   1                    [OK]
microsoft/iis           Internet Information Services (IIS) instal...   1         [OK]       [OK]
microsoft/mongodb       MongoDB installed in a Windows Server Core...   1                    [OK]
microsoft/mysql         MySQL installed in a Windows Server Core b...   1                    [OK]
microsoft/nginx         Nginx installed in a Windows Server Core b...   1                    [OK]
microsoft/node          Node installed in a Windows Server Core ba...   1                    [OK]
microsoft/php           PHP running on Internet Information Servic...   1                    [OK]
microsoft/python        Python installed in a Windows Server Core ...   1                    [OK]
microsoft/rails         Ruby on Rails installed in a Windows Serve...   1                    [OK]
microsoft/redis         Redis installed in a Windows Server Core b...   1                    [OK]
microsoft/ruby          Ruby installed in a Windows Server Core ba...   1                    [OK]
microsoft/sqlite        SQLite installed in a Windows Server Core ...   1                    [OK]
microsoft/nano-golang   Go Programming Language installed in a Nan...   1                    [OK]
microsoft/nano-httpd    Apache httpd installed in a Nano Server ba...   1                    [OK]
microsoft/nano-iis      Internet Information Services (IIS) instal...   1         [OK]       [OK]
microsoft/nano-mysql    MySQL installed in a Nano Server based con...   1                    [OK]
microsoft/nano-nginx    Nginx installed in a Nano Server based con...   1                    [OK]
microsoft/nano-node     Node installed in a Nano Server based cont...   1                    [OK]
microsoft/nano-python   Python installed in a Nano Server based co...   1                    [OK]
microsoft/nano-rails    Ruby on Rails installed in a Nano Server b...   1                    [OK]
microsoft/nano-redis    Redis installed in a Nano Server based con...   1                    [OK]
microsoft/nano-ruby     Ruby installed in a Nano Server based cont...   1                    [OK]
```

To download an image from Docker Hub, use `docker pull`.

```none
docker pull microsoft/aspnet

Using default tag: latest
latest: Pulling from microsoft/aspnet
f9e8a4cc8f6c: Pull complete

b71a5b8be5a2: Download complete
```

The image will now be visible when running `docker images`.

```none
docker images

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
microsoft/aspnet    latest              b3842ee505e5        5 hours ago         101.7 MB
windowsservercore   10.0.10586.0        6801d964fda5        2 weeks ago         0 B
windowsservercore   latest              6801d964fda5        2 weeks ago         0 B
```

