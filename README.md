# TensorFlow-GPU

Tensorflow Version: 1.6

Example script to run job on MCW-RCC GPU cluster:
```
#!/bin/bash
#PBS -N tensorflow_test
#PBS -S /bin/bash
#PBS -l nodes=1:ppn=1:gpus=1
#PBS -l mem=5gb
#PBS -l walltime=72:00:00
#PBS -j oe

module load tensorflow/1.6                    # Load the TensorFlow software

cd $PBS_O_WORKDIR
export SCR_DIR=/scratch/global/$USER/$PBS_JOBID
mkdir -p $SCR_DIR
cp my_tf_script.py $SCR_DIR                   # Copy all input files to scratch directory
cd $SCR_DIR
tensorflow-gpu my_tf_script.py >> output.log  # run your tensorflow python script
cp -R output.log $PBS_O_WORKDIR               # Copy output back to working directory
rm -rf $SCR_DIR                               # Clean up your scratch directory
```
