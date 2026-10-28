# Shared GPU server: VS Code setup

Run notebook cells on our **NVIDIA RTX 2060** server with **TensorFlow 2.17.0**, while editing in VS Code on your own computer.

## 1. Connect to Tailscale

1. Install [Tailscale](https://tailscale.com/download) and sign in.
2. Open the shared server invitation sent privately by the server owner and accept it using the same account.
3. Keep Tailscale connected while using the server. See [Tailscale's sharing guide](https://tailscale.com/docs/features/sharing) if needed.

## 2. Install the VS Code extensions

Open **Extensions** in VS Code (`Ctrl + Shift + X`) and install **Python** and **Jupyter**, both published by **Microsoft**.

## 3. Open your notebook

Open the project folder, then open a Python notebook such as `Model/ResNet_Fruits.ipynb`.

## 4. Connect to Jupyter

Before setting up the Kernal, setup your .env file first. It is as simple as getting your
access token and filling in the missing text

**Create your OWN .env file do NOT update the .env_template as it will commit a secret to the repo**

1. Click **Select Kernel** at the top right. If a kernel is already selected, click its name.
2. Choose **Select Another Kernel…** if shown, then **Existing Jupyter Server**.
3. Choose **Enter the URL of the running Jupyter server** and enter:
   **!IMPORTANT!**
   Make sure you input the URL like this, if not VS Code will keep reprompting you for
   a password and it will not connect to the Kernal
   ```text
   https://hades.taild3c5cd.ts.net:8889/?token=<YOUR_ACCESS_TOKEN>
   ```

4. For the password it is the token that the admins sends
   ```

You can ask the admin to send the token in an ecrypted link.

Keep the actual token out of notebook cells, Git commits and screenshots. These steps follow the [VS Code remote kernel guide](https://code.visualstudio.com/docs/datascience/jupyter-kernel-management#_existing-jupyter-server).

## 5. Start your own remote kernel

Select the server's **Python 3 (ipykernel)** entry (or similarly named Python 3 kernel) to start a **new session**. Avoid selecting another person's running notebook session. A kernel is the Python process that runs your cells.

## 6. Check TensorFlow and the GPU

Run this as the **first TensorFlow cell after starting your kernel**, using `Shift + Enter`:

```python
import tensorflow as tf

gpus = tf.config.list_physical_devices("GPU")

# Allocate GPU memory as needed instead of reserving most of it at startup.
for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)

print("TensorFlow:", tf.__version__)
print("GPUs detected:", gpus)
assert gpus, "No GPU detected: check your remote kernel and contact the server owner."
```

Expect version **2.17.0** and a non-empty GPU list containing `GPU:0`. An empty list (`[]`) means TensorFlow cannot see the GPU. If memory growth reports that the GPU is already initialized, restart your kernel and run this cell first. See [TensorFlow's GPU guide](https://www.tensorflow.org/guide/gpu).

## 7. Terminate your session when finished

1. Save your notebook in VS Code (`Ctrl + S`) and save any trained model you want to keep. Shutting down clears the kernel's variables and any unsaved model held in memory.
2. Keep Tailscale connected and open [the Jupyter server](https://hades.taild3c5cd.ts.net:8889/) in your browser. Enter the private token if prompted.
3. Open **Running Terminals and Kernels** in the JupyterLab left sidebar, or the **Running** tab if the server shows the classic Notebook interface.
4. Find **your notebook's session** and click its **Shut Down** button (or the shutdown icon beside it), then confirm. If you cannot identify your session, ask the server owner before stopping anything.
5. Check that your kernel disappears from the running list. You can then close VS Code or disconnect Tailscale.

This stops your Python process and releases its GPU memory. **Closing a tab, disconnecting, or interrupting a cell does not reliably shut down the kernel.** Avoid **Shut Down All**, which would stop other group members' work. See [Jupyter's session management guide](https://jupyterlab.readthedocs.io/en/stable/user/running.html).

## Working together

- Everyone can connect simultaneously with their own kernels and separate Python variables, but **GPU memory and computing power are shared**. Memory growth does not impose a per-person limit; coordinate large training runs or reduce batch sizes if memory runs out.
- Shut down your own kernel when finished to release its GPU memory; closing the notebook tab may leave it running.
- Code executes on the server. Dataset paths must point to files on the server; local paths such as `C:\Users\...` are not automatically uploaded.
- If the connection fails, check Tailscale is connected, the invitation was accepted, and the URL and private token are correct.
