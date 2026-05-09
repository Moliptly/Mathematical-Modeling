#### 作业要求
1. 熟练使用神经网络的工具箱。
2. 了解BP神经网络的训练算法，结合最优化算法中的无约束优化方法理解标准BP算法的改进算法.

#### 实现过程
- **工具箱使用**
	- 导入数据集设置输入信号，输入信号![[1720177930988.png]]
	- ![[1720177973655.png]]
	- 设置输出类型为数值矩阵，框选输入输出数据
	- 命令行输入nftool
	- 导入输入输出数据
	- ![[1720178053741.png]]
	- ![[1720178079201.png]]
	- ![[1720178094237 1.png]]
	- 选择训练方法即可
	- ![[1720178195125.png]]
	- 可以生成各种训练状态图
- matlab代码实现+无约束优化
	- 在上述工具箱中点击生成代码（综合训练脚本）
```matlab
% 解决输入-输出拟合问题的神经网络脚本  
% 脚本由神经拟合应用程序生成  
% 创建时间：2024年7月5日 19:18:02  
%  
% 此脚本假定以下变量已定义：  
%  
%   input - 输入数据。  
%   output - 目标数据。  
  
x = input'; % 输入数据  
t = output'; % 目标数据  
  
% 选择训练函数  
% 要查看所有训练函数的列表，请输入：help nntrain  
% 'trainlm' 通常是最快的。  
% 'trainbr' 需要更长时间，但对于复杂问题可能更好。  
% 'trainscg' 使用更少的内存，在内存有限的情况下适用。  
trainFcn = 'trainlm';  % 利用Levenberg-Marquardt反向传播算法进行训练。  
  
% 创建拟合网络  
hiddenLayerSize = 10;  
net = fitnet(hiddenLayerSize,trainFcn);  
  
% 选择输入和输出的预处理/后处理函数  
% 要查看所有处理函数的列表，请输入：help nnprocess  
net.input.processFcns = {'removeconstantrows','mapminmax'};  
net.output.processFcns = {'removeconstantrows','mapminmax'};  
  
% 设置数据划分为训练、验证和测试  
% 要查看所有数据划分函数的列表，请输入：help nndivision  
net.divideFcn = 'dividerand';  % 随机划分数据  
net.divideMode = 'sample';  % 每个样本划分  
net.divideParam.trainRatio = 70/100;  % 训练集比例  
net.divideParam.valRatio = 15/100;   % 验证集比例  
net.divideParam.testRatio = 15/100;  % 测试集比例  
  
% 选择性能函数  
% 要查看所有性能函数的列表，请输入：help nnperformance  
net.performFcn = 'mse';  % 均方误差  
  
% 选择绘图函数  
% 要查看所有绘图函数的列表，请输入：help nnplot  
net.plotFcns = {'plotperform','plottrainstate','ploterrhist', ...  
    'plotregression', 'plotfit'};  
  
% 训练网络  
[net,tr] = train(net,x,t);  
  
% 测试网络  
y = net(x);  
e = gsubtract(t,y);  
performance = perform(net,t,y);  
  
% 重新计算训练、验证和测试的性能  
trainTargets = t .* tr.trainMask{1};  
valTargets = t .* tr.valMask{1};  
testTargets = t .* tr.testMask{1};  
trainPerformance = perform(net,trainTargets,y);  
valPerformance = perform(net,valTargets,y);  
testPerformance = perform(net,testTargets,y);  
  
% 查看网络结构  
view(net);  
  
% 绘图  
% 取消注释以下行以启用各种图形。  
% figure, plotperform(tr)  
% figure, plottrainstate(tr)  
% figure, ploterrhist(e)  
% figure, plotregression(t,y)  
% figure, plotfit(net,x,t)  
  
% 部署  
% 将 (false) 改为 (true) 以启用以下代码块。  
% 查看每个生成函数的帮助以获取更多信息。  
if (false)  
    % 生成 MATLAB 函数以供应用程序部署  
    genFunction(net,'myNeuralNetworkFunction');  
    y = myNeuralNetworkFunction(x);  
end  
if (false)  
    % 生成仅矩阵的 MATLAB 函数以进行 MATLAB Coder 工具的代码生成  
    genFunction(net,'myNeuralNetworkFunction','MatrixOnly','yes');  
    y = myNeuralNetworkFunction(x);  
end  
if (false)  
    % 生成 Simulink 图表以进行仿真或部署  
    gensim(net);  
end
```

- 无约束优化参见 [[最优化理论与技术]无约束优化方法 - ColleenHL - 博客园 (cnblogs.com)](https://www.cnblogs.com/ColleenHe/p/11782538.html)
- ![[1720178629519.png]]
	- matlab中可以直接生成量化共轭训练方法（一种无约束优化方法）
	- 这里选择直接从代码中自行加入优化，代码如下：
```matlab
% 解决输入-输出拟合问题的神经网络脚本  
% 脚本由神经拟合应用程序生成  
% 创建时间：2024年7月5日 19:18:02  
%  
% 此脚本假定以下变量已定义：  
%  
%   input - 输入数据。  
%   output - 目标数据.  
  
% 输入数据和目标数据  
x = input';  
t = output';  
  
% 选择训练函数  
trainFcn = 'trainlm';  % 利用Levenberg-Marquardt反向传播算法进行训练。  
  
% 优化目标函数（例如：均方误差的负值，因为 fminunc 是最小化函数）  
objFunc = @(hiddenLayerSize) -trainNNAndGetMSE(hiddenLayerSize, trainFcn, x, t);  
  
% 初始隐藏层神经元数量  
initialHiddenLayerSize = 10;  
  
% 设置 fminunc 的选项  
options = optimoptions('fminunc', 'Display', 'iter', 'MaxIterations', 20);  
  
% 使用 fminunc 进行优化  
bestHiddenLayerSize = fminunc(objFunc, initialHiddenLayerSize, options);  
  
% 创建拟合网络  
net = fitnet(bestHiddenLayerSize, trainFcn);  
  
% 其余代码保持不变  
net.input.processFcns = {'removeconstantrows','mapminmax'};  
net.output.processFcns = {'removeconstantrows','mapminmax'};  
net.divideFcn = 'dividerand';    
net.divideMode = 'sample';    
net.divideParam.trainRatio = 70/100;    
net.divideParam.valRatio = 15/100;     
net.divideParam.testRatio = 15/100;    
net.performFcn = 'mse';    
  
% 训练网络  
[net,tr] = train(net,x,t);  
  
% 测试网络    
y = net(x);    
e = gsubtract(t,y);    
performance = perform(net,t,y);    
    
% 重新计算训练、验证和测试的性能    
trainTargets = t .* tr.trainMask{1};    
valTargets = t .* tr.valMask{1};    
testTargets = t .* tr.testMask{1};    
trainPerformance = perform(net,trainTargets,y);    
valPerformance = perform(net,valTargets,y);    
testPerformance = perform(net,testTargets,y);    
    
% 查看网络结构    
view(net);    
    
% 绘图    
% 取消注释以下行以启用各种图形。    
% figure, plotperform(tr)    
% figure, plottrainstate(tr)    
% figure, ploterrhist(e)    
% figure, plotregression(t,y)    
% figure, plotfit(net,x,t)    
    
% 部署    
% 将 (false) 改为 (true) 以启用以下代码块。    
% 查看每个生成函数的帮助以获取更多信息。    
if (false)    
    % 生成 MATLAB 函数以供应用程序部署    
    genFunction(net,'myNeuralNetworkFunction');    
    y = myNeuralNetworkFunction(x);    
end    
if (false)    
    % 生成仅矩阵的 MATLAB 函数以进行 MATLAB Coder 工具的代码生成    
    genFunction(net,'myNeuralNetworkFunction','MatrixOnly','yes');    
    y = myNeuralNetworkFunction(x);    
end    
if (false)    
    % 生成 Simulink 图表以进行仿真或部署    
    gensim(net);    
end  
  
% 辅助函数：训练神经网络并返回均方误差  
function mse = trainNNAndGetMSE(hiddenLayerSize, trainFcn, x, t)  
    net = fitnet(hiddenLayerSize, trainFcn);  
    net.input.processFcns = {'removeconstantrows','mapminmax'};  
    net.output.processFcns = {'removeconstantrows','mapminmax'};  
    net.divideFcn = 'dividerand';    
    net.divideMode = 'sample';    
    net.divideParam.trainRatio = 70/100;    
    net.divideParam.valRatio = 15/100;     
    net.divideParam.testRatio = 15/100;    
    net.performFcn = 'mse';    
    [net,~] = train(net,x,t);  
    y = net(x);  
    mse = perform(net,t,y);  
end
```

#### 作业结果
- 网络结构  ![[1720179169817.png]]
- 训练状态![[1720179085211.png]]
- 性能![[1720179098361.png]]
- 误差直方图![[1720179111574.png]]
- 回归![[1720179295259.png]]
- 拟合
