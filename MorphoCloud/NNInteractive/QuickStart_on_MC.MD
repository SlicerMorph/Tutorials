## Run NNinteractive without installing any libraries:

We maintain a python virtual environment all necessary files for running the NNInteractiver server is install. For quick tests, it might be faster to use this environment instead of creating one for yourself as explained in the NNInteractive readme.  There are a few downsides of using this environment: 

1. If the share is not available, you can't use it
2. It is a read-onl folder, you cannot make changes if things don't work out for you.

As such, if you are planning to use NNInteractive regularly, we still suggest creating your own python virtual environment where you have the full control over. 

Here are the quick instructions.
1. First paste this command in a terminal window to map the share that contains NNinteractive virtual environment (one time setup):
```
curl https://jetstream2.exosphere.app/exosphere/assets/scripts/mount_ceph.py | sudo python3 - mount \
  --access-rule-name="NNInteractive-ro" \
  --access-rule-key="AQCMQTxpTikzNxAAssyTNC8+spC8seJJVnZ+BQ==" \
  --share-path="149.165.158.38:6789,149.165.158.22:6789,149.165.158.54:6789,149.165.158.70:6789,149.165.158.86:6789:/volumes/_nogroup/b27879c9-bfd4-4290-a87e-91a3dad87107/55c933bc-5291-450d-997c-ee407a05c740" \
  --share-name="NNInteractive"
```
2. In the same window, type this command to activate the environment
```
source /media/share/NNInteractive/bin/activate
```
3. Start the NNInteractive server: 
```
nninteractive-slicer-server --host 0.0.0.0 --port 1527
```

For subsequent usages, you do not need to do the step 1. Just activate your environment and start the server.
