# Contributing Guide

Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are greatly appreciated.

## Documentation

1. Clone [→ the repository](https://github.com/swifdroid/docs) and open it in VSCode.
2. Hit `Ctrl/Cmd + Shift + P` and select `Run Task -> Pull Material Docker Image`.
3. Hit `Ctrl/Cmd + Shift + P` and select `Run Task -> Run Material Server`.
4. Open [0.0.0.0:8000](http://0.0.0.0:8000) in your browser to preview the documentation.
5. Make your changes in the markdown files located in the `docs/` folder.
6. Once you are satisfied with your changes, commit and push them.
7. Open a pull request to the main repository for review.

## Droid

1. Clone [→ the repository](https://github.com/swifdroid/droid) and open it in VSCode.
2. Make your changes in the codebase.
3. Once you are satisfied with your changes, commit and push them.
4. Open a pull request to the main repository for review.

## JNIKit

1. Clone [→ the repository](https://github.com/swifdroid/jni-kit) and open it in VSCode.
2. Make your changes in the codebase.
3. Once you are satisfied with your changes, commit and push them.
4. Open a pull request to the main repository for review.

## Runtime Libraries

Each Swift release has its own set of runtime libraries that need to be maintained and updated.

The process of updating the runtime libraries takes few manual steps:

1. Find the link to the latest Swift Android SDK release.
2. Execute `./copy-so-files.sh <SDK-URL>` to download and extract the shared libraries.
3. Commit and push the changes.

If you want to help with this process, please reach out!  
This process can be automated in the future, feel free to help with that.


**Thank you for considering contributing to Swift for Android!**

!!! success ""
    Don't forget to give the project a star! Thanks again!