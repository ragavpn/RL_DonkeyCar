# RL_DonkeyCar
Train an RL agent in a simulator known as DonkeyCar optimized using an autoencoder for faster training.

### Setup:

- **Simulator**: Download the simulator from [here](https://github.com/tawnkramer/gym-donkeycar/releases/download/v22.11.06/DonkeySimLinux.zip) to set up connection with the libraries and the client.

```bash
# Extract the folder and run
chmod +x DonkeySimLinux/donkey_sim.x86_64

# Now to launch the simulator just run
./DonkeySimLinux/donkey_sim.x86_64 
```
<br>

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
cd Autoencoder
pip install -e .
```
<br>

- **Auto Encoder**: Code is given below to collect data for auto encoder and to train it.

```bash
# Go to the folder
cd Autoencoder

# To collect data, run this and drive around using keys in the simulator for two laps without hitting any edge and resetting the environment
python record_data.py -f logs/dataset-mountain -n 10000

# To train the autoencoder using the data collected
python -m ae.train_ae --n-epochs 500 --batch-size 8 --z-size 32 -f logs/dataset-mountain/ --verbose 1

# To continue the training (use the best version till then in logs folder)
python -m ae.train_ae --n-epochs 500 --batch-size 8 --z-size 32 -f logs/dataset-mountain/ --verbose 1 -ae logs/<your_best>.pkl
```
<br>

- **Training**: Code is given below to train the agent and analyse the statistics using tensorboard.

```bash
# Go to sb3 folder
cd Client

# Start training without tensorboard and modifying parameters
python train.py --algo tqc --env donkey-mountain-track-v0 --eval-freq -1 --save-freq 25000

# Start training with tensorboard and modifying parameters
python train.py --algo tqc --env donkey-mountain-track-v0 --eval-freq -1 --save-freq 25000 --save-replay-buffer -tb /tmp/sb3/ -params learning_starts:500

# Continue Training (Use the latest needed version in the command)
python train.py --algo tqc --env donkey-mountain-track-v0 -i logs/tqc/<your_best>/rl_model_200000_steps.zip -n 25000 --eval-freq -1 --save-freq 25000 --save-replay-buffer -tb /tmp/sb3/ -params learning_starts:500

# Access Tensorboard
tensorboard --logdir /tmp/sb3/
```
<br>

- **Errors**: Few errors while training, if faced, below is a quick fix.
  
```bash
# If any import error(UnregisteredEnv) occurs with respect to environment copy contents from
./Gym_Donkeycar_env/
# to (replace with your profile name)
/home/<your_profile>/anaconda3/envs/donkey/lib/python3.8/site-packages/gym_donkeycar/
```
<br>
