# RL_DonkeyCar
A simulation environment for a Reinforcement-Learning Based car

### Setup:

- **Simulator**: Download the simulator from [here](https://github.com/tawnkramer/gym-donkeycar/releases/download/v22.11.06/DonkeySimLinux.zip) to set up connection with the libraries and the client.

```bash
# Extract the folder and run
chmod +x DonkeySimLinux/donkey_sim.x86_64

# Now to launch the simulator just run
./DonkeySimLinux/donkey_sim.x86_64 
```

- **Conda Environment**: Skip the first two lines if you have already installed conda.

```bash
# Download anaxonda3
curl -O https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh

# Install anaconda3 (follow the on-screen instructions)
bash ./Anaconda3-2024.02-1-Linux-x86_64.sh

# Create new environment named 'donkey' with necessary specifications
conda create -n donkey python=3.8
conda activate donkey
conda install pytorch=1.11.0 torchvision torchaudio cudatoolkit=11.3 -c pytorch
conda install -c conda-forge gym=0.21.0 seaborn=0.11.2 pyyaml=5.4.1 opencv=4.5.5
pip install gym-donkeycar==1.3.1 stable-baselines3==1.5.0 sb3-contrib==1.5.0 optuna==2.10.0 optuna[stable-baselines3] pyzmq==22.3.0 pygame==2.1.2 imgaug==0.4.0 joblib==1.1.0 tensorboard==2.8.0 protobuf==3.20.0 ipython==7.31.0 pillow==10.3.0
cd aae-train-donkeycar
pip install -e .

```
- **Auto Encoder**: Code is given below to collect data for auto encoder and to train it.

```bash
# Go to the folder
cd aae-train-donkeycar

# To collect data, run this and drive around using keys in the simulator for two laps without hitting any edge and resetting the environment
python record_data.py -f logs/dataset-mountain -n 10000

# To train the autoencoder using the data collected
python -m ae.train_ae --n-epochs 500 --batch-size 8 --z-size 32 -f logs/dataset-mountain/ --verbose 1

# To continue the training
python -m ae.train_ae --n-epochs 500 --batch-size 8 --z-size 32 -f logs/dataset-mountain/ --verbose 1 -ae logs/ae-32_1716979559_best.pkl
```

