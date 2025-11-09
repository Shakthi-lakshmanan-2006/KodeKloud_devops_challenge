# Day 72 : Jenkins Parameterized Builds

## 📌 Basics to Know Before Starting

Before we begin configuring parameterized builds in Jenkins, it’s important to understand a few basic concepts:

### 🔹 Jenkins Freestyle Job
A Freestyle Job in Jenkins is a general-purpose build job. It allows you to define build steps manually and is commonly used for simple build and deploy tasks.

### 🔹 Parameterized Builds
Parameterized builds allow users to pass different values when running a job. This lets you build the same job for different environments or configurations without modifying the job itself.

### Types of Parameters Used Here:
| Parameter Type | Name | Purpose | Example |
|---------------|------|----------|---------|
| **String Parameter** | `Stage` | To store text input | `Build` |
| **Choice Parameter** | `env` | To choose from multiple predefined values | `Development`, `Staging`, `Production` |

---
![Screenshot 1](./assets/Screenshot%202025-10-25%20213253.png)

---

## 🛠 Steps to Create the Parameterized Job

### 1. Login to Jenkins
- Username: `admin`
- Password: `Adm!n321`

### 2. Create the Job
1. Click **New Item**.
2. Enter the job name: **`parameterized-job`**
3. Select **Freestyle Project** → Click **OK**.

### 3. Enable Parameters
1. Check the option: **This Project is Parameterized**
2. Add a **String Parameter**:
   - Name: `Stage`
   - Default value: `Build`
3. Add a **Choice Parameter**:
   - Name: `env`
   - Choices:
     ```
     Development
     Staging
     Production
     ```

### 4. Add a Shell Build Step
Go to **Build Steps → Execute Shell** and enter the following script:

```bash
echo "This is Stage parameter value : $Stage"
echo "This is env parameter value : $env"
```

### 5. Run the Build
* Click **Build with Parameters**
2. Select `env = Staging`
3. Click **Build**

---
![Screenshot 2](./assets/Screenshot%202025-10-25%20213208.png)

---

## 🧾 Summary of Commands

| Purpose | Command |
|--------|---------|
| Print Stage Parameter | `echo "This is Stage parameter value : $Stage"` |
| Print env Parameter | `echo "This is env parameter value : $env"` |

---

✅ **You have successfully created a Parameterized Jenkins job!**  
You can now deploy to different environments using the same job just by selecting parameter values.

---

![Screenshot 3](./assets/Screenshot%202025-10-25%20213339.png)

---