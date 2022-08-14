# Tutorial: Install and run *mesh_fusion* on a local PC (tested on Ubuntu20.0)
modified mesh fusion for personal use

Source: https://github.com/davidstutz/mesh-fusion

## Step 0 -- Create a python 3.7 environment
create an environment
```
conda create -n environment_name python=3.7.4
```
activate the environment
```
conda activate environment_name
```
## Step 1 -- Basic Installation
### Install *glut* and *opengl*
```
sudo apt install freeglut3-dev mesa-utils
```
### Install *GLEW* (The OpenGL Extension Wrangler Library)
```
sudo apt-get install libglew-dev
```
### Install *cmake*
```
sudo apt install cmake
```
### Install *cython*, *h5py*, *scipy*
```
conda install -c anaconda cython=0.29.2 h5py=2.9.0 scipy=1.1.0
```
### Install *pymeshlab*
```
pip3 install pymeshlab
```
## Step 2 -- Install and build *mesh_fusion_cpu*
### Install *mesh_fusion*
```
git clone https://github.com/UncleTom111/mesh_fusion_cpu.git
```
### Build *mesh_fusion*
#### build pyfusion
```
cd mesh_fusion_cpu
cd libfusioncpu
mkdir build
cd build
cmake ..
make
cd ..
python setup.py build_ext --inplace
cd ..
```
#### build pyrender
```
cd librender
python setup.py build_ext --inplace
cd ..
```
#### build PyMCubes
```
cd libmcubes
python setup.py build_ext --inplace
cd ..
```
### Modify *mesh_fusion*
!!!! The name of  generated ```.so``` files are various in different PC and environment. !!!!
- Step 1 -- find thsese ```.so``` files under directories ***librender*** and ***libfusioncpu***
- Step 2 -- change the file name below according to the ```.so``` files you found
- example: ```pyrender.cpython-38-x86_64-linux-gnu.so```
```
cp -i ./librender/pyrender.cpython-37m-x86_64-linux-gnu.so ./pyrender.so
cp -i ./libfusioncpu/cyfusion.cpython-37m-x86_64-linux-gnu.so ./cyfusion.so
```
## Step 3 --  Run mesh_fusion (demo)
### Scale an Object
```
python 1_scale.py --in_dir=examples/0_in/ --out_dir=examples/1_scaled/
```
### Render depth views
```
python 2_fusion.py --mode=render --in_dir=examples/1_scaled/ --depth_dir=examples/2_depth/ --out_dir=examples/2_watertight/
```
### make mesh watertight
```
python 2_fusion.py --mode=fuse --in_dir=examples/1_scaled/ --depth_dir=examples/2_depth/ --out_dir=examples/2_watertight/
```
### make mesh watertight and simplified
```
python 3_simplify.py --in_dir=examples/2_watertight/ --out_dir=examples/3_out/
```
