# PyStrategies: Framework for Algorithmic Trading Strategy Research

## Toolset

### Backtesting

        ./bin/backtest.sh quotes/XOM_BATS_2010-12-09.csv strategy/pyStrategy.py

### Graphing Results

        # Generate book snapshots - make sure CREATE_ON_NBBO_CHANGE_ONLY is set appropriately
        ./PyLimitBook/create_graphing_data.py ./quotes/XOM_BATS_2010-04-09.csv ./analyze/data/book.csv

        # Move signals generated by backtest
        mv ./strategy/signals_log.csv ./analyze/data/signals.csv

        jupyter notebook --notebook-dir=./analyze

### Strategy Parameter Optimization

        # Specify parameter ranges in optimize/parameters.csv
        ./optimize/optimize.py quotes/XOM_BATS_2010-12-09.csv strategy/pyStrategy.py 1000

### Machine Learning

        # Generate features
        ./machine_learning/generate_features.py quotes/XOM_BATS_2010-12-09.csv

        # Generate labels to predict
        ./machine_learning/generate_labels.py quotes/XOM_BATS_2010-12-09.csv

        # Train the model
        ./machine_learning/train_nn.py quotes/XOM_BATS_2010-12-09.csv

        # Test the model
        ./machine_learning/test_nn.py quotes/XOM_BATS_2010-12-10.csv

## Installation

### Installing CuDNN

1. Download CuDNN: https://developer.nvidia.com/rdp/cudnn-download
  * Following configuration works:
    * nvcc version: release 7.5, V7.5.26
    * cuda driver: 7.5.27
    * clang version: clang-703.0.29 (XCODE 7.3.0)
    * osx version: 10.11.4
2. Install libraries:
  *  cd <installpath>
  *  Linux: export LD_LIBRARY_PATH=`pwd`:$LD_LIBRARY_PATH
  *  Mac: export DYLD_LIBRARY_PATH=`pwd`:$DYLD_LIBRARY_PATH
       * Copied *.h files to CUDA_ROOT/include and *.so* files to CUDA_ROOT/lib64
           * By default, CUDA_ROOT is /usr/local/cuda on Linus and /Developer/NVIDIA/CUDA-* on mac
       * Added the following to the end of my ~/.profile:

             export PATH="/Developer/NVIDIA/CUDA-7.5/bin:$PATH"
             export DYLD_LIBRARY_PATH="/Developer/NVIDIA/CUDA-7.5/lib:$DYLD_LIBRARY_PATH"

  *  Add <installpath> to your build and link process by adding -I<installpath> to your compile line and -L<installpath> -lcudnn to your link line.
3. Create a ~/.theanorc file: (skip markdown backticks!)

        [global]
        device=gpu
        floatX=float32
        allow_gc=False
        warn_float64=warn

        [lib]
        cnmem=0.50

        [nvcc]
        fastmath=True

### Install jpy

1. git clone https://github.com/bcdev/jpy.git
2. cd jpy
3. export JDK_HOME=`/usr/libexec/java_home`
4. export JAVA_HOME=$JDK_HOME
5. python setup.py --maven build
6. Copy lib/jpy-0.8.jar to the Java project
7. Copy properties file into the same directory as the Java project:
  * build/lib.*/jpyconfig.properties