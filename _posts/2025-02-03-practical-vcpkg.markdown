---
layout: post
title:  "A practical setup of vcpkg for company use"
date: 2025-02-03
categories: C++
tags: C++ vcpkg
---

This is a practical use case for a companies engineering organization

There are many blogs and postings (e.g. redit, stackoverflow) about how to use the many different pieces
of vcpkg, but not many (if any) that give a real-world example of how to setup an environment for company
to manage their C/C++ dependencies across teams and projects. This post aims to fill that gap by giving a
full setup of what that might look like.

Assumptions?
- you understand basics of vcpkg (manifest mode)

This is how the company I work for uses vcpkg, there could be better/other ways, but this is the one we
currently use and it works.

## Project Repository Setup
The most basic thing to do is setup a project (CMake based) to use vcpkg as the package manager.

```cmake
cmake_minimum_required (VERSION 3.25)

# Optionally, uncomment to load triplets specific to project
# set (VCPKG_OVERLAY_TRIPLETS "${CMAKE_SOURCE_DIR}/triplets")

# Optionally use a vcpkg root specified by an environment variable. An
# example of where this is useful is for builds of a source tree that is
# mounted into a docker container (e.g. keeps your non-docker environment
# clean of build files).
if (DEFINED ENV{VCPKG_ROOT})
  set (
    CMAKE_TOOLCHAIN_FILE
    "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
    CACHE STRING "Vcpkg toolchain file"
  )
else ()
  set (
    CMAKE_TOOLCHAIN_FILE
    "./vcpkg/scripts/buildsystems/vcpkg.cmake"
    CACHE STRING "Vcpkg toolchain file"
  )
endif ()

# Configure project
project (...)

# Remainder of cmake configuration...

```

setup vcpkg configuration to use builtin so that we are not constantly cloning vcpkg
## Setup a company registry
Once a company grows beyond a mono-repo they will want to share libraries among their different project
repositories. To facilitate this a vcpkg registry can be created to shared company specific dependencies.
This repository can be either public or private.

what types of projects go into this registry?
- modification to public ports (easiest)
- company developed libraries

## Setup binary cache

## Setup a local registry


### Using local registry
