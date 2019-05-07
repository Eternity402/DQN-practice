# DQN-practice
This repo is for getting used to DQN.

1. 1. training 횟수 줄여보기 : default(2000) / exp = 1000 : 어느 정도 학습은 되는데 학습이 완벽히 되지는 않았음. 
조금 특별한 점이라면 학습이 거의 다 된 것처럼 보이다가 뭔가 예외사항을 발견하고 그 뒤로 조금 성능이 떨어지는 듯한 경향을 보였음. 
근데 이게 특이 케이스가 아니고 꽤 높은 확률로 발생하는 듯. 
아마도 cartpole-v0의 두 가지 패배조건인 수직으로부터 15이상 떨어지는 것과 가운데로부터 2.4이상 멀어지는 것 중 후자를 학습하는 과정에서 발생하는 문제인 듯 싶다. 
후반에 끝나는 경우는 죄다 2.4이상이어서 종료되는 경우뿐이므로, 이 경우 training이 부족해서 발생하는 문제(새로운 규칙에 맞닥뜨리게 되므로)임이 분명하다. 
근데 학습이 안되는 경우도 존재하긴 하는 듯. 1500 넣고 한번 돌려봤는데 학습이 안됨. randomness에 따라서 성능이 차이가 많이 갈리는 것 같음. 
근데 신기한 점은 1500회로 놓고 돌렸을 때보다 1000회로 놓고 돌린 경우 더 학습이 잘되는 경향을 보였음
(확실히 1000회로 놓고 했을 때가 성능이 더 좋음 - 1500회는 애초에 200차 episode까지 살아남지 못함)

2. 2. discount factor 값 바꿔보기 : default(0.99) / exp = 1 : cartpole-v0의 경우 확실히 reward를 제공하고, 
reward 또한 discount가 되지 않기 때문에 이 경우 discount factor 없이 그냥 1로 두었을 때 더 학습이 잘 되는 것 같음.
확실히 cartpole 자체에는 random noise가 포함될 요소가 없기 때문이기도 할 듯. 
(이 다음부터는 빨리 결과 보고 싶기도 해서 그냥 training 횟수 1000에 discount factor 1 놓고 실험함)

3. 3. node 수 바꿔보기 : default(layer 1개 & 16 nodes) / exp = (layer 1개 & 2 nodes) : training 횟수가 적었을 때 처럼 학습이 되다가 안 되었음. 
그래서 training 횟수를 늘려서 한 번 실험해보았음. 그래도 일정 수준 이상으로는 성능이 올라가지 않음. 계속 upper bound를 툭툭 건드리는 느낌이랄까? 
exp = (layer 1개 & 4 nodes) : 이 경우 default 때의 성능과 유사했음. 그러나, 수렴 속도가 살짝 늦은 것으로 보였음. exp = (layer 1개 & 32 nodes) : 
수렴속도가 빨랐으나, 두 번째 game over조건을 아직 습득하지 못함. 이건 training 횟수를 늘리면 해결될 문제인 것 같긴 함. 오히려, default의 case에서는 
두 번째 game over 조건을 제대로 학습한 것이 아닐 수도 있겠다는 생각이 듬. 왜냐면, default의 경우 최고 성능에 도달하고 나서 감소하는 구간이 없었지만, 
이 경우 살짝 감소하는 부분을 지났기 때문에 default case에서는 확실한 training인지 생각해봐야 할듯.(물론 10번 돌린 결과 최고성능을 기록하긴 했었긴 함)
exp = (layer 1개 & 256 nodes) : 이 경우는 32 case 보다도 훨씬 빠른 수렴속도를 가졌지만, 마찬가지로 두 번째 game over 조건을 학습하지 못했음. 
이는 experience replay의 특성 때문으로 해석할 수 있을 것 같음. exp = (layer 1개 & 1024 nodes) : 놀랍게도 학습이 거의 안되는 것으로 보임. 
개인적으로 생각해볼 때 이유는 high dimension에서 정답 label space로 찾아가기에 training 횟수가 부족한듯. 
training 횟수 늘려보니 실제로 성능도 늘어나긴 함.

4. 4. layer 늘려보기 :  default(layer 1개 & 16 nodes) / exp = (layer 2개 & 2:2 nodes) : 
음... 개인적으로 기대했던 건 layer 1개 & 4 nodes와 비슷한 수렴속도와 성능이었는데, 수렴속도는 일단 월등히 낮았음. 
성능도 training 횟수 올려서 확인해 봤는데, node가 부족했을 때처럼 성능도 일정 bound를 넘지 못하는 것처럼 보임. 
exp = (layer 2개 & 4:4 nodes) : 마치 node가 엄청 많은 경우처럼 학습속도도 엄청 빨랐고, game over 2번째 조건에 쩔쩔매는 모습을 보였음. 
exp = (layer 2개 & 8:2 nodes) : 학습이 엄청 불안정했지만, 어쨌든 두 번째 game over 조건을 학습하는 단계까지는 도착했음. 
다시 해보니까 엄청 불안정한 정도는 아니지만, 그래도 안정적이라고 볼 수는 없을 정도임. 
exp = (layer 2개 & 2:8 nodes) : 8:2 일 때보다 학습이 안정적임. 
exp = (layer 3개 & 2:2:2 nodes) : layer 2개 4:4 nodes와 마찬가지의 성능과 수렴속도를 보였음. 거의 유사했음. 

5. 5. activation 바꿔보기 : default(leaky_relu) /
exp = (relu) : relu보다 수렴속도나 성능이 확실히 떨어짐. 
exp = (sigmoid for 1, leaky_relu for 2) : sigmoid 때문인지 학습이 거의 안되는 모습을 보임. 
exp = (tanh for 1, leaky_relu for 2) : 압도적인 성능임. 지금까지 1000회 기준 training 했던 모델중 가장 좋은 성능을 보였음. 중간에 성능이 압도적으로 오르는 case가 있었음. 
여러번 test해봐도 그래프가 비슷하게 나오는 걸 보면 굉장히 잘 작동하는 것 같음. 그러나 마찬가지로 결국 game over 2번째 조건은 아직 해결하지 못한 듯. 이건 이 cartpole-v0의 특성이라고 생각함. 특수성을 지니긴 했지만, 다른 env에서도 분명히 한 layer정도는 끼워넣어 봐도 좋을 거라고 생각함.
exp = (swish) : 거의 학습이 안되는 경향을 보였음. 이유는 잘은 가늠이 안됨. 
exp = (swish for 1, leaky_relu for 2) : tanh와 leaky_relu를 섞었을 때와 비슷한 성능을 보였으나, 그보다는 살짝 떨어졌음. 아마도 env마다 성능이 다르게 나오는 것으로 예상됨. 
(이 뒤로 activation default는 tanh & leaky_relu 조합으로 하였음 - 너무 느려서 다시 leaky_relu로 바꿨음..) 

6. 6. 앱실론 값 조정 : default(from start to half, 1 to 0.1) / 
exp = (from start to end, 1 to 0.1) : 후반에 가서 성능이 꽤 올라가긴 했지만, 원래보다는 성능이 떨어지는 것이 보였음. / 
exp = (from start to half, 0.5 to 0.1) : 학습이 초반에 빨리 되는 경향을 보였으나, 약간 불안정했음. 추측하기에 exploration이 부족해서 생기는 문제로 보임. 
아직 맞닥뜨리지 못한 state와 action pair에 대해서 학습이 되지 않아 발생하는 것이 아닐까 짐작함. 하지만, 최종적으로 2000을 달성하였으므로 주어진 횟수 안에 training을 잘 마무리한 것으로 생각할 수 있음. 

7. 7. learning rate 조정 : default(0.001) / 
exp = (0.01) : 이 case에서는 학습이 더 빨리 되는 장점을 보였음. 그러나 확실히 한계가 존재하는듯? 학습이 잘 되는 case도 있었는데 뭔가 벽에 부딪히고 다시 추락했음. 
아? 새로운 game over 조건을 발견하고 나서 너무 그쪽으로 많이 치우쳐서 이런 상황이 발생하는 것으로 보임! 
뿐만 아니라 bound에 걸려서 앞으로 나아가지를 못함. 
exp = (0.1) : 0.01과 비슷한 양상. 엄청 불안정하고 그런 경향이 있음. 
exp = (0.0001) : 이 경우 1000회는 약간 부족한 듯. 성능이 올라가는 도중에 training이 종료되었음. 

8. 8. minibatch size 조정 : default(50) / 
exp = (128) : 수렴 속도가 살짝 빠름. 그러나 training이 살짝 느려진 것 같음(기분 탓일수도). 
그렇지만 성능도 좋고 두 번째 game over 조건도 무난하게 학습함. 완벽한 것 같지는 않음. 
exp = (16) : 뭐 큰차이는 없음. exp = (2) : 굉장히 불안정함. 초반에 성능이 좋은 case가 있었으나 다시 추락하고 올라오는데 속도가 걸림(완전하지도 않았음) training 횟수를 늘리면 좋아질 것 같긴 함. 
