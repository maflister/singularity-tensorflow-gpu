# TensorFlow-GPU
[![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/1269)

Tensorflow Version: 1.10

## Batch Job Script
```
#!/bin/bash
#PBS -N tensorflow_test
#PBS -S /bin/bash
#PBS -l nodes=1:ppn=1:gpus=1
#PBS -l mem=5gb
#PBS -l walltime=72:00:00
#PBS -j oe

module load tensorflow/1.10                    

cd $PBS_O_WORKDIR
export SCR_DIR=/scratch/global/$USER/$PBS_JOBID
mkdir -p $SCR_DIR
cp my_tf_script.py $SCR_DIR                   
cd $SCR_DIR
tensorflow-gpu my_tf_script.py >> output.log  
cp -R output.log $PBS_O_WORKDIR              
rm -rf $SCR_DIR                             
```
1. Copy contents of tf.pbs (example above) to a file in your home directory.

2. Open terminal on cluster login node and submit the job script:

```
$ qsub tf.pbs
```

## TensorFlow Jupyter Notebook Job Script
```
#!/bin/bash
#PBS -N tf_jnb
#PBS -l nodes=1:ppn=1:gpus=1
#PBS -l mem=5gb
#PBS -l walltime=1:00:00
#PBS -j oe

# get tunneling info
port=$(shuf -i8000-9999 -n1)
node=$(hostname -s)
user=$(whoami)
submit_host=$(echo ${PBS_O_HOST%%.*}.rcc.mcw.edu)
cd $PBS_O_WORKDIR

# print tunneling instructions jupyter-log
echo -e "
1. SSH tunnel from your workstation using the following command:

   ssh -L ${port}:${node}:${port} ${user}@${submit_host}

   and point your web browser to http://localhost:${port}.

2. Copy/paste the token below when you connect for the first time.
"

# load modules
module load tensorflow/1.10

jupyter-notebook
```
1. Copy contents of tfjupyternb.pbs (example above) to a file in your home directory.

2. Open terminal on cluster login node and submit the job script:

```
$ qsub tfjupyternb.pbs
```

### Connect to Notebook
1. Check output file (*jobname*.o*JOBNUM*) for details.

Example output file: tf_jnb.o152668
```
1. SSH tunnel from your workstation using the following command:

   ssh -L 9623:node01:9623 tester@loginnode

   and point your web browser to http://localhost:9623.

2. Copy/paste the token below when you connect for the first time.

[I 18:24:35.251 NotebookApp] Serving notebooks from local directory: /home/tester/jupyter-test
[I 18:24:35.251 NotebookApp] 0 active kernels
[I 18:24:35.251 NotebookApp] The Jupyter Notebook is running at:
[I 18:24:35.251 NotebookApp] http://node01:9623/?token=a2caff9717f5dcefc22c4c387fa0a02390d1ef71f0f3747d
[I 18:24:35.251 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 18:24:35.256 NotebookApp]

    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://node01:9623/?token=a2caff9717f5dcefc22c4c387fa0a02390d1ef71f0f3747d
```

2. Open second terminal and run tunneling command from the output file:
```
$ ssh -L 9623:node01:9623 tester@loginnode
```
3. Open a browser and enter the URL from the output file:
```
http://localhost:9623
```
4. Enter the token (long string after ?token=) from the output file.

You should now be connected to your Jupyter Notebook that is running on a cluster compute node. To close the notebook, select Logout. If you need to reconnect, repeat steps. If you're done with your Notebook, remember to stop the job with qdel.


## TensorBoard Job Script
```
#!/bin/bash
#PBS -N tensorboard
#PBS -l nodes=1:ppn=1:gpus=1
#PBS -l mem=5gb
#PBS -l walltime=1:00:00
#PBS -j oe

# get tunneling info
port=$(shuf -i8000-9999 -n1)
node=$(hostname -s)
user=$(whoami)

# add your own log directory (required)
logdir="~/path/to/your/logdir"

cd $PBS_O_WORKDIR

# print tunneling instructions 
echo -e "
1. SSH tunnel from your workstation using the following command:

   ssh -L ${port}:${node}:${port} ${user}@$PBS_O_HOST

   and point your web browser to http://localhost:${port}.

2. Copy/paste the token below when you connect for the first time.
"

# load modules
module load tensorflow/1.10

tensorboard-gpu
```
1. Copy contents of tensorboard.pbs (example above) to a file in your home directory. Modify the logdir variable.

2. Open terminal on cluster login node and submit the job script:

```
$ qsub tensorboard.pbs
```

### Connect to TensorBoard
1. Check output file (*jobname*.o*JOBNUM*) for details.

Example output file: tensorboard.o178936
```
SSH tunnel from your workstation using the following command:

   ssh -L 8754:node01:8754 tester@loginnode

and point your web browser to http://localhost:8754.
```

2. Open second terminal and run tunneling command from the output file:
```
$ ssh -L 8754:node01:8754 tester@loginnode
```
3. Open a browser and enter the URL from the output file:
```
http://localhost:8754
```

You should now be connected to TensorBoard. To close TensorBoard, close browser window. If you need to reconnect, repeat steps.
