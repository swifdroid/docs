# Setup on Windows

## Docker

Install Docker for Windows by following the instructions from [here](https://docs.docker.com/desktop/setup/install/windows-install/).

> Choose the WSL 2 backend when prompted during installation.

## VSCode

Download and install Visual Studio Code from [here](https://code.visualstudio.com/download).

Once installed, open VSCode and head to extensions tab (Ctrl+Shift+X) to install the following extensions:

[Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

![VSCode with DevContainers extension installed](../assets/screenshots/devconatiners-extension-in-list.webp "VSCode with DevContainers extension installed")

> When you try to create your first project, the Dev Containers extension may ask to download additional Docker components, even though Docker is already installed. Allow it to do so.

[Swift Stream IDE](https://marketplace.visualstudio.com/items?itemName=swiftstream.swiftstream)

![VSCode with Swift Stream IDE extension installed](../assets/screenshots/swift-stream-extension-in-list.webp "VSCode with Swift Stream IDE extension installed")

Aaaaaand... you are all set!

Now check out our [→ Application](app/index.md) and [→ Library](lib/index.md) guides.