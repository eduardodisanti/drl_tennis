# Tennis

Trains two colaborative tennis agents using Multi Agent Deep Deterministic Policy Gradient (MADDPG)

<center>
    [<img src="https://img.youtube.com/vi/J38OPprZrbI/maxresdefault.jpg" width="50%">]
</center>

## Introduction

This project shows how to train the reacher multiple environment in the Udacity modified (from Unity's Reacher) environment.<br/>
The goal of the agent is to mantain the ball in the air as much time as possible using a two joint arm.<br/>
This project trains many agents at a time to maintain the a ball on the air, applying torque to the two joints.<br/>
Check [Report file](https://github.com/eduardodisanti/drl_tennis/blob/master/report/Report_Tennis.pdf) for technical specifications, algorithm, hyperparameters and architecture of the neural network.<br/>
A reward is granted when any agent reaches the ball in the correct position and none otherwise.

## Files description

|  File | Description | Location |
|-------|-------------|----------|
| Tennis.ipynb  | Notebook that trains the model and track progress | / |
| ddpgagent.py  | Defines the DQN agent to solve the environment. | /code |
| models.py  | Defines the Agent and Critic Neural Networks. | /code |
| train_agent_headless.py  | Contains code for training the agent that can be run standalone | /code |
| Report_Tennis.pdf  | Technical paper and training report | /report |
| actor_chk.pt | Saved weights of the <b>actor</> neural network | /model
| critic_chk.pt | Saved weights of the <b>critic</b> neural network | /model
| actor_chk.pt.best | Saved weights of the <b>best actor</> neural network weights| /model
| critic_chk.pt.best | Saved weights of the <b>best critic</b> neural network weights| /model
| tennis_video.m4v | A video showing the trained agent | /media

## Instructions 
*Refer to "Setting up the environment" section to download the environment for this project in case you don't have the Unity environment installed.

### Training the agent

- Training in a Jupyter notebook (recommended), 
	Open the Reacher_multi notebook and run all cells <br/>
	During the training a graph will be displayed, updated at the end of every episode.<br/>
  <img src="https://github.com/eduardodisanti/drl_tennis/blob/master/report/training_1000.png" width="180"/><br/>
	At the end of the training a final figure with appear showing the rewards, average, moving average and goal<br/>
	<center>
		<img src="https://github.com/eduardodisanti/drl_tennis/blob/master/report/training_1000.png" alt="training report" width="180"/>
	</center>
	<br/>
	Episode 100 score mean 2.3750000363215804 average on deque 1.9352000317443163<br/>
    100 2.3750000363215804 0.5 100<br/>
    Environment SOLVED in 100episodes<br/>
    Moving Average = 1.9352000317443163 over last 100 episodes<br/>
	<br/>
- Training from the command line,
	Execute *cd code*<br/>
	Run the train_agent_headless.py using the command *python train_agent_headless.py*<br/>
	During the training the progress of the training will be written to the console. <br/>

The training will stop when the agent reach an average of +0.5 on last 100 episodes. (Will happen around 20 episodes)<br/>
Model will be saved every time the score improves (when the episode ends) in a separate files (.best) <br/> 

## Apendices

### Agent characteristics
*Please check [Report file](https://github.com/eduardodisanti/drl_tennis/blob/master/report/Report_Tennis.pdf) for a better understanding of the algorithm*
### Agent
#### Algorithm
The agent uses an Actor-Critic approach with Deep Deterministic Policy Gradient algorithm
#### Actor
The neural network that estimate the action-value function for the Actor has following architecture:

|  Layer | Neurons Size  | Type | Activation | Comment |
|--------|-------|------|------------|---------|
|Input  |  2 | | | according to the space state dimension | 
|Hidden 1  |  256 | Linear | ReLU |
|Hidden 2  |  256 | Linear | ReLU |
|Output  |  2 | Linear | TanH | One for each action between -1 and 1
The  optimization algorithm used was **Adam**

#### Critic
The neural network that estimate the action-value function has following architecture:

|  Layer | Neurons Size  | Type | Activation | Comment |
|--------|-------|------|------------|---------|
|Input  |  2 | | | according to the space state dimension | 
|Hidden 1  |  512 + 2 | Linear | ReLU | plus action vector size
|Hidden 2  |  256 | Linear | ReLU |
|Output  |  1 | Linear | ReLU | Action value
The  optimization algorithm used was **Adam**

##### Hyperparameters
-   Discount factor, $\gamma$ 0.99
-   Soft-update ratio, $\tau$     0.001
-   Network update interval
    -  Local every **4** time steps.
    -  Target every **4** time steps.
-   Replay buffer size  **1e6**
-   Minibatch size **128**
-   &epsilon; configuration and decay rate for **&epsilon;-greedy policy**
    -   Start          : 1
    -   Minumin.  : 0.01
    -   Decay rate: 1e-6
-   Learning rate for ACTOR: 1e-3
-   Learning rate for CRITIC: 1e-4
-   L2 weight decay = 0
-   Learn every: 20 timesteps
-   Number of learning passes: 10
-   Ornstein-Uhlenbeck noise parameters
    -   Sigma noise parameter: 0.2
    -   Theta noise parameter: 0.15

With the above hyperparameters, the average score of the last 100 episodes reached 36.74 after 100 episodes.

<br/>
And the average reward given for 20 agents over 100 episodes was:
<table>
<tr>
<td><img src="https://github.com/eduardodisanti/drl_tennis/blob/master/report/training_1000.png" width="480"/></td>
<tr>
</table>

#### Future work
Check the possibility to consider the environment solved before, maybe some hyper parameter tuning can allow the agent to converge faster

#### Setting up the environment
1. Download the environment from one of the links below.  You need only select the environment that matches your operating system:
    - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip)
    - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis.app.zip)
    - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86.zip)
    - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86_64.zip)
    
    (_For Windows users_) Check out [this link](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64) if you need help with determining if your computer is running a 32-bit version or 64-bit version of the Windows operating system.<br/>
    (_For AWS_) If you'd like to train the agent on AWS (and have not [enabled a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md) to obtain the environment.

2. Then, place the files and folders of this repository in the `p3_collab-compet/` folder in the DRLND GitHub repository, you can overwrite any file in conflict.  Next, open `Tennis.ipynb` to train the agent.
