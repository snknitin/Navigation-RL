# Report


## Learning Algorithm

DQN 
* I had the batch_size as 128 and UPDATE_EVERY as 16. Tuning other hyperparameters might help converge even faster

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
* If environment could be modified, then **shaping the rewards** might help improve the performancce of the agent as well. 
