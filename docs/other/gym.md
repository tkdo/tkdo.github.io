# CartPole 
- 描述：在这个游戏中，有一辆小车，上面有一个竖直放置的杆子。玩家需要控制小车的移动，使得杆子保持在竖直方向上，尽可能多的时间。当杆子偏离竖直太多或者小车移动到边缘时，游戏结束。
- observation:状态
- action: 0 1 代表左右移动
例如 CartPole-v0 环境，它的目标是尽可能长时间地保持杆子竖直，没有明确的胜利条件，只有失败条件，即小车偏离中心或者杆子倾斜太多。因此，在 CartPole-v0 环境中，您可以一直玩下去，只要您的小车不偏离中心或者杆子不倾斜太多。

envs中的__init__.py
```python
register(
    id="CartPole-v0",
    entry_point="gym.envs.classic_control.cartpole:CartPoleEnv",
    max_episode_steps=200, # >>>>> 这里表示最优也就200步，做的最好了
    reward_threshold=195.0,
)
```
### 机器随机玩一轮
```python
import gym
done = False # 是否结束
score = 0
env = gym.make("CartPole-v0") # 初始化环境
state = env.reset() #环境重置
while not done: 
    action = env.action_space.sample() #随机输入action
    observation, reward, done, info = env.step(action)
    print(f"done:{done}, reward:{reward}, observation:{observation}")
    score = score + reward
print(score) #最后得分
```
### 手动玩一轮CartPole
```python 
import gym
done = False # 是否结束
score = 0
env = gym.make("CartPole-v0") # 初始化环境
state = env.reset() #环境重置
while not done: 
    x = input("Enter 0 or 1: ")
    x = if x not in ["0", "1"] else "0" #获取输入action
    observation, reward, done, info = env.step(action=int(x))
    print(f"done:{done}, reward:{reward}, observation:{observation}")
    score = score + reward
print(score) #最后得分
```


### 为什么需要2个模型?
    玩贪吃蛇的时候，如果目标也在移动游戏就会变得困难。
    在训练时，用经验评估动作模型的目标。
    如果目标总是在变，训练就会变得困难不稳定。
    所以需要暂时让目标确定下来，达到一个目标后，在追求下一个目标。


