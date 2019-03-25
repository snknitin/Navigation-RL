# Report


## Learning Algorithm - Value based methods  - DQN

The Deep Q-Learning algorithm represents the optimal action-value function as a neural network. Unfortunately, **reinforcement learning is notoriously unstable when neural networks are used to represent the action values**. 

* **Experience Replay**: When the agent interacts with the environment, the sequence of experience tuples can be highly correlated. The naive Q-learning algorithm that learns from each of these experience tuples in sequential order runs the risk of getting swayed by the effects of this correlation. By instead **keeping track of a replay buffer and using experience replay to sample from the buffer at random, we can prevent action values from oscillating or diverging catastrophically**.

    * The replay buffer contains a collection of experience tuples (S, A, R, S'). The tuples are gradually added to the buffer as we are interacting with the environment.
    * The act of sampling a small batch of tuples from the replay buffer in order to learn is known as experience replay. In addition to breaking harmful correlations, experience replay allows us to learn more from individual tuples multiple times, recall rare occurrences, and in general make better use of our experience.
      
We are breaking the correlation between consecutive experience tuples by sampling randomly out of order.
      

* **Fixed Q-Targets** : we update a guess with a guess, and this can potentially lead to harmful correlations. Just like a horse moving towards a dangling carrot on a stick in front of it , never reaches it, the Q-values are isntrinsically tied to through the function parameters. Here the correlation is between the target and the parameters we are changing. Literally, chasing a moving target. Each action affects the exact position of the target in a very complicated and unpredictable manner. If we decouple the target's position from the actions then we have a more stable learning environment andd this is done by fixing the function parameters used to generate our target. J is the MSE between the Q values - TD target(R+γ max_a(Q(s',a,w-)) and current estimate Q(s,a,w) where w- are the weights of a separate target network that is not changed during the learning step

   * Target Q network, which is identical, but the weights are updated less often or slowly
   
## Algorithm

There are two main processes involved. One is where we sample the environment by performing actions and store away the observed experienced tuples in the replay buffer, the other is where we select a small random batch from these tuples and learn using gradient descent update step.  These aren't directly dependent on each other, so you can perform multiple samplings and then one learning step, or even multiple learning steps with different random batches. 

* In the beginning, we initialize an empty array or circular queue to retain the n most recent experience tuples.
* Initialize parameters or weights of the NN, like sampling randomly from normal distribution
* To use the fixed q target technique we need a second set of pramaters w-
* There is usually a preprocessing step involved with the games with raw images - grayscale, cropping. In order to capture temporal relationships we can stack a few input frames to build each state vector. A sequence of frames is combined into a representation. If we want to stack 4 frames, the first 3 timesteps in the first episode timestep, we can treat the missing 3 as blank or use copies of the 4th. We can skip storing tuples till we have a complete sequence.
* We need to wait to have sufficient replay before learning. We don't clear out the memory after each episode. This enables us to recall and build batches of experiences from across episodes.

**Improvements**:

* **Double DQN**

In the early stages the Q values are still evolving and we may not have gathered enough data to determine the best action but we choose the action which maximizes the Q value. **Since we take the max out of noise values, Deep Q-Learning tends to overestimate action values**. Double Q-Learning has been shown to work well in practice to help with this. **We select the best set action using one set of parameters W but evaluate it using a different set of parameters W'. It is basically like having 2 separate function approximators that must agree on the best action.In the long run this prevent the algo from propagating indcidental high rewards that may have been obtained by chance and don't reflect long term returns**. W- from the fixed target can also be used.


* I had the **batch_size as 128 and UPDATE_EVERY as 16**. Tuning other hyperparameters might help converge even faster

Rest of the details of the algorigthm can be found [here](https://github.com/udacity/deep-reinforcement-learning/tree/master/dqn)

## Plot of Rewards


          Episode 100	Average Score: 1.12
          Episode 200	Average Score: 4.57
          Episode 300	Average Score: 8.04
          Episode 400	Average Score: 10.33
          Episode 495	Average Score: 13.00
          Environment solved in 395 episodes!	Average Score: 13.00
            
            

![alt text](https://github.com/snknitin/Navigation-RL/blob/master/curve.PNG)


## Ideas for Future Work



* I would like to try looking at the effects of prioritized replay and Dueling DQN. 6 different improvements to DQN are implemenmted in RAINBOW.
* I would also like to see how significant the difference is between different algorithmns and find a way to compare them
* If environment could be modified, then **shaping the rewards** might help improve the performancce of the agent as well. Based on the number of blue and number of yellow bananas collected, we can have a higher negative penalty for blue that keeps increasing with the number of collected banana. if the percentage of yellow bananas from the total_collected_so_far is > 70% then give a higher scaled reward.
