# Setup on Linux

## Docker

Install Docker Desktop for Linux by following the instructions from [here](https://docs.docker.com/desktop/setup/install/linux/).

> Arch users knows what to do even without any instructions.

## VSCode

Download and install Visual Studio Code from [here](https://code.visualstudio.com/download) or via your package manager.
> If your VSCode installation is sandboxed (i.e. via flatpak), you'll need to uninstall and download directly from the [official website](https://code.visualstudio.com/download).
> Otherwise, Dev Containers won't have access to Docker.

Once installed, open VSCode and head to extensions tab (Ctrl+Shift+X) to install the following extensions:

[Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

![VSCode with DevContainers extension installed](../assets/screenshots/devconatiners-extension-in-list.webp "VSCode with DevContainers extension installed")

> When you try to create your first project, the Dev Containers extension may ask to download additional Docker components, even though Docker is already installed. Allow it to do so.

[Swift Stream IDE](https://marketplace.visualstudio.com/items?itemName=swiftstream.swiftstream)

![VSCode with Swift Stream IDE extension installed](../assets/screenshots/swift-stream-extension-in-list.webp "VSCode with Swift Stream IDE extension installed")

Aaaaaand... you are all set!

Now check out our [→ Application](../app/index.md) and [→ Library](../lib/index.md) guides.
