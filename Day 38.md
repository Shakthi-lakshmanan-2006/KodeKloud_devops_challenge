# Day 38 Challenge: Docker Image Tagging

## 📝 Task

Nautilus project developers are planning to start testing on a new
project. As per their meeting with the DevOps team, they want to test
containerized environment application features.

**Task:**\
- Pull the `busybox:musl` image on **App Server 2** in Stratos DC.\
- Re-tag (create new tag) this image as `busybox:local`.
 
------------------------------------------------------------------------

## 🧑‍💻 Beginner Basics You Should Know


### 1. What is Docker?

Docker is a platform to **package applications** into small, portable
containers. Think of a container as a "mini-computer" that runs exactly
what you tell it to -- no extra stuff.

-   **Images** → Blueprints (like a recipe for a cake).\
-   **Containers** → Running instances of those images (like the actual
    cake baked from the recipe).

------------------------------------------------------------------------

### 2. What is an Image Tag?

An image in Docker has two parts:

    <image-name>:<tag>

Examples:\
- `nginx:latest` → nginx image, latest version.\
- `busybox:musl` → busybox image, musl variant.\
- `busybox:local` → busybox image, local tag (we'll create this).

If no tag is given, Docker assumes **`:latest`** by default.

👉 A tag is just a **label** for an image, nothing more. You can have
multiple tags pointing to the same underlying image.

------------------------------------------------------------------------

### 3. What is BusyBox?

-   BusyBox is a **very small Linux distribution** -- basically a
    lightweight command-line toolbox.\
-   It's used often in testing containers because it's **tiny** and
    loads fast.\
-   The `:musl` variant means it's built with the musl C library (an
    alternative to glibc).

For our purpose: we're just downloading it, not running any app.

------------------------------------------------------------------------

### 4. Docker Commands You Need to Know

1.  **Pull an image**

    ``` bash
    docker pull <image-name>:<tag>
    ```

    → Downloads the image from Docker Hub to your system.

2.  **List images**

    ``` bash
    docker images
    ```

    → Shows all images stored locally.

3.  **Tag an image**

    ``` bash
    docker tag <source-image>:<source-tag> <target-image>:<target-tag>
    ```

    → Creates another "alias" (tag) for the same image.

    Example:

    ``` bash
    docker tag busybox:musl busybox:local
    ```

    → Now the same image can be used either as `busybox:musl` or
    `busybox:local`.

------------------------------------------------------------------------

## 🚀 Step-by-Step Solution

### Step 1: Log in to App Server 2

Confirm you are on the correct server:

``` bash
hostname
```

### Step 2: Pull the busybox:musl Image

``` bash
docker pull busybox:musl
```

### Step 3: Verify Image Download

``` bash
docker images
```

### Step 4: Re-tag the Image

``` bash
docker tag busybox:musl busybox:local
```

### Step 5: Verify the New Tag

``` bash
docker images
```

------------------------------------------------------------------------

<img width="1907" height="1027" alt="Screenshot 2025-09-16 140035" src="https://github.com/user-attachments/assets/b4908466-d71d-4d54-ad4f-b04568a8b9dc" />

## ✅ Summary of Commands

``` bash
docker pull busybox:musl  
docker images  
docker tag busybox:musl busybox:local  
docker images  
```

------------------------------------------------------------------------
<img width="1596" height="901" alt="Screenshot 2025-09-16 140117" src="https://github.com/user-attachments/assets/dbb1d7fb-4f97-42e1-80ee-bc8383895fa1" />
