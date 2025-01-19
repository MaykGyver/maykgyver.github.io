---
layout: post
title: Install Apache Directory Studio on Windows using winget only
date: "2025-01-19"
---

## Summary

```powershell
winget install -e Apache.DirectoryStudio
winget install -e EclipseAdoptium.Temurin.21.JDK
```

## Explanation

When it comes to the choice of a desktop operating system and environment, there is a lot of potential for debates about ideologies and likings. One of the more measurable differences was the availability of mature package managers on Linux (i.e. `apt` on Ubuntu) and a holistic farrago on Windows. Until `winget` entered the stage. One may complain about Microsoft's business strategy, but from a architectural and technical perspective, they did the best job they could do in that situation. Today, only very exotic software that I require is not in [the main winget repository](https://github.com/microsoft/winget-pkgs) (yet). Some of the packages with trivial dependencies handle them very well already.

Today I had my first attempt to install Apache Directory Studio (ADS) since adopting winget. It requires a java virtual machine (JVM) which commonly is packaged in a java runtime environment (JRE) or a java development kit (JDK). If you happen to already have any reasonable JVM installed, you invoke `winget install -e Apache.DirectoryStudio` and you're ready to go. But if you did not have a JVM installed previously, ADS will tell you.

![Screenshot of Apache Directory Studio's error message dialog. It states: A Java Runtime Environment (JRE) or Java Development Kit (JDK) must be available in order to run ApacheDirectoryStudio. No Java virtual machine was found after searching the following locations.](/assets/apache-directory-studio-no-java-virtual-machine.png)

If it was trivial to decide, which JVM to install, I'd love to write a small PR on github instead of [an elaboration in a natural language](.). But my experience with java folks tells me, that they are quiet sensitive when you mess with their precious configuration of installed JVMs. So let's take a look at [Apache Directory Studio's Download for Windows page](https://directory.apache.org/studio/download/download-windows.html). In its Requirements section it points to [Adoptium's home page](https://adoptium.net). The most prominent button there suggests you that you probably may want its LTS release currently at 21.0.5.

![Screenshot of the most prominent button on Adoptium's home page. It states: Latest LTS Release jdk-21.0.5+11](/assets/adoptium-download-button-lts-21.0.5.png)

This translates to `winget install -e EclipseAdoptium.Temurin.21.JDK`. The next approach to start Apache Directory Studio should work now.

If there was a fairy that offered me three free wishes, they would be:
1. A winget package with id `EclipseAdoptium.Temurin.LTS.JDK` that simply points to the current LTS release of Eclipse Adoptium Temurin Java Runtime Environment. So we won't need to lookup the current state of the products' life cycles when we come back later.
2. Good luck for winget to grow and mature dependency handling over the next two years or so.
3. Peace on earth. But that's another story.
