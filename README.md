# 🚀 Kubernetes Init Container with Service Dependency — `kp07` Namespace

This project demonstrates how to use **init containers** in Kubernetes to delay the start of a main container until a dependent service (`myservice`) is available.

---

## 📁 Step 1: Create a Separate Namespace
We begin by creating a dedicated namespace for our work called `kp07`.

![image](https://github.com/user-attachments/assets/2e7ad796-cf79-4edd-bac8-2cc81e063cd3)


![image](https://github.com/user-attachments/assets/7a7a2d55-64ff-4cbd-9498-3ff9195e45e3)

## 🧱 Step 2: Deploy Pod with Init and Main Containers

We use a YAML file named `Kaustubh.yaml` to create a pod that has **two containers**:

- 🟡 **Init Container**: Runs first to check if `myservice` is resolvable using `nslookup`.
- 🔵 **Main Container**: Runs only after the init container completes successfully.

---


![image](https://github.com/user-attachments/assets/c577f1c6-43c0-41c9-99e5-53dc70d6068f)

## 🔍 Step 3: Init Container Logic

The init container uses the following command:

[ until nslookup myservice; do echo "Waiting for myservice..."; sleep 2; done ]


This ensures that the pod’s main container will only run after the DNS resolution for myservice is successful.

![image](https://github.com/user-attachments/assets/7b1032aa-4823-401c-ad90-dade0f257dd2)

##  **Step 4: Pod Fails to Start – Event Log**

After creating the pod, we inspect the logs and observe an error in the Events tab:

Init container failed: cannot resolve myservice

This happens because myservice doesn't exist yet in the cluster.

![image](https://github.com/user-attachments/assets/783d6c05-746f-44d0-8632-9297bc970222)

![image](https://github.com/user-attachments/assets/96929c66-95fe-43ef-8b56-08c0b3f2c737)

## 🛠️ **Step 5: Create and Expose a Deployment as myservice**

To resolve this issue, we create a deployment (e.g., NGINX or any HTTP server) and expose it as a Kubernetes Service named myservice on port 80.

[ kubectl expose deployment my-deployment --port=80 --name=myservice ]
 
![image](https://github.com/user-attachments/assets/f8cd12b6-77f9-4287-8eaf-fbc1f0674c82)

## **✅ Step 6: Init Container Resolves myservice Successfully**

Once the service is available, the init container completes its DNS check successfully, and the main container starts as expected.

![image](https://github.com/user-attachments/assets/27ba68d2-ddd8-4ebe-9d42-985b2939363b)

![image](https://github.com/user-attachments/assets/adf10084-2697-4a07-a0b6-34466b5bb1b3)

## **📦 Files Included**

Kaustubh.yaml — Pod spec with init and main containers

deployment.yaml — Deployment file to create myservice

namespace.yaml — Namespace definition (kp07)

## **📸 Screenshots**

Refer to the above screenshots for your reference to view terminal outputs, YAML configurations, and Kubernetes Dashboard views

## **🙌 Conclusion**

This setup demonstrates how init containers can be used to control pod startup based on the availability of dependent services — a useful pattern in microservice deployments.

## **⭐ Feel free to fork, clone, and contribute!**
