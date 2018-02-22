这是以前自己读研时的一个比较简单的项目，分享出来，希望能帮到一些对神经网络和模式识别感兴趣的人。
#基于BP神经元网络算法的模式识别
## BP神经元网络算法

  - 神经网络是对人脑或自然神经网络的若干基本特性的抽象，是一种基于连接假说构造的智能仿生模型，人们试图通过对它的研究最终揭开人脑的奥秘，建立起能模拟人脑功能和结构的智能系统，使计算机能够像人脑那样进行信息处理。本文主要用基于BP神经元网络算法进行模式识别[11]。
  - 我们以三层BP网络为例，介绍该算法。它的结构如图 所示，包含一个输入层、一个隐含层和一个输出层，分别由个n,p,q神经元组成。BP神经网络结构如图8所示。
  ![这里写图片描述](http://img.blog.csdn.net/20180106170902782?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  ![这里写图片描述](http://img.blog.csdn.net/20180106170911991?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  ![这里写图片描述](http://img.blog.csdn.net/20180106170925763?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  ![这里写图片描述](http://img.blog.csdn.net/20180106170939702?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1YmluMTk5MWxpdWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 

##GitHub项目源码
- GitHub链接：https://github.com/RobinLiew/wireless-communication  这里面有测试数据，详细的介绍文档，源码，介绍了利用BP神经元网络算法对无线信道进行模式识别的案例。
- 核心源码

```
function  Bpmodeldistinguish( X,Y )

%*****************************************************
%利用matalb实现基于BP神经网络模式识别实现代码
%*****************************************************

% X是三个场景的平均特征向量所组成的矩阵（就是fea_average）
% Y是待匹配的特征向量（就是b1或b2）
%**************************************************************************
%**************** 先对X和Y进行标准化处理 *******************
%**************************************************************************
X=[1e9*X(1,:);1e3*X(2,:);X(3,:)/100];
X=X';
X0=ones(3,16);
X0(:,[1:3])=X;
X=X0;
Y=[1e9*Y(1);1e3*Y(2);Y(3)/100];
Y=Y';
Y0=ones(1,16);
Y0(1:3)=Y;
Y=Y0;


%**************************************************************************
%*******************主函数main*********************************************
%*************************START********************************************
%**************************************************************************
Y1=[1 0 0];      %输出模式           
Y2=[0 1 0];
Y3=[0 0 1];
Yo=[Y1;Y2;Y3]; %输出的三种模式
n=16; %输入层神经元个数
p=8;  %中间层神经元个数
q=3;  %输出神经元个数
k=3 ;%训练模式个数
a1=0.2; b1=0.2; %学习系数，
%rou=0.5;%动量系数，
emax=0.01; cntmax=100;%最大误差，训练次数
[w,v,theta,r,t,mse]=bptrain(n,p,q,X,Yo,k,emax,cntmax,a1,b1);%调用函数bptrain训练网络
c=bptest(p,q,n,w,v,theta,r,Y);
c(c>0.5)=1;
c(c<0.2)=0;
disp('测试数据的识别结果:')
c
if (min(c==Y1)==1)
    fprintf('该数据和场景1匹配\n');
elseif (min(c==Y2)==1)
        fprintf('该数据和场景2匹配\n');
else
        fprintf('该数据和场景3匹配\n');
end

%**************************************************************************
%**************************END*********************************************
%**************************************************************************

    function [w,v,theta,r,t,mse]=bptrain(n,p,q,X,Yo,k,emax,cntmax,a1,b1)
%n表示输入神经元个数,p表示中间层神经元个数，q表示输出神经元个数,
%X表示输入训练模式，Yo表示标准输出，k表示训练模式的个数
%emax表示最大误差，cntmax表示最大训练次数，a1，b1表示学习系数，rou表示动量系数
%w、theta表示训练结束后输入层与中间层连接权系数和阈值，
%v、r表示训练结束后中间层与输出层连接权和阈值
%t表示训练时间
tic
w=rands(n,p);%输入层与隐含层连接权
v=rands(p,q); %隐含层与输出层连接权
theta=rands(1,p);%中间层的阈值
r=rands(1,q);%输出层的阈值
cnt=1;
mse=zeros(1,cntmax);%全局误差为零
er=0;
while ((er>emax)|(cnt<=cntmax))
 E=zeros(1,q);
 %循环识别模式  
 for cp=1:k
     X0=X(cp,:);             
     Y0=Yo(cp,:);

     %计算中间层的输入Y(j) 
     Y=X0*w; 
     %计算中间层的输出b
     Y=Y-theta;    %中间层阈值
     for j=1:p
         b(j)=1/(1+exp(-Y(j)));%中间层输出f(sj)
     end      
    %计算输出层输出c
             Y=b*v;
             Y=Y-r;  % 输出层阈值
        for t=1:q
           c(t)=1/(1+exp(-Y(t))); %输出层输出
        end 
    %计算输出层校正误差d
        for t=1:q 
          d(t)=(Y0(t)-c(t))*c(t)*(1-c(t));
        end
   %计算中间层校正误差e
         xy=d*v';
         for t=1:p
           e(t)=xy(t)*b(t)*(1-b(t));
         end
   %计算下一次的中间层和输出层之间新的连接权v(i,j),阈值t2(j)
          for t=1:q
              for j=1:p
                  v(j,t)=v(j,t)+a1*d(t)*b(j);
              end
              r(t)=r(t)+a1*d(t);
          end
      %计算下一次的输入层和中间层之间新的连接权w(i,j),阈值t1(j)
           for j=1:p
              for i=1:n
                  w(i,j)=w(i,j)+b1*e(j)*X0(i);
              end
              theta(j)=theta(j)+b1*e(j);
           end
           for t=1:q
               E(cp)=(Y0(t)-c(t))*(Y0(t)-c(t))+E(cp);%求当前学习模式的全局误差
           end
           E(cp)=E(cp)*0.5;
    %输入下一模式    
  end
  er=sum(E);%计算全局误差
  mse(cnt)=er;
  cnt=cnt+1;%更新学习次数
 end
t=toc;


function c=bptest(p,q,n,w,v,theta,r,X)
%p,q,n表示输入层、中间层和输出层神经元个数
%w，v表示训练好的神经网络输入层到中间层、中间层到输出层权值。
%X为输入测试的模式
%c表示模式X送入神经网络的识别结果。
%计算中间层的输入Y(j) 
Y=X*w; 
%计算中间层的输出b
Y=Y-theta;    %中间层阈值
for j=1:p
    b(j)=1/(1+exp(-Y(j)));%中间层输出f(sj)
end      
%计算输出层输出c
Y=b*v;
Y=Y-r;  % 输出层阈值
thr1=0.01;thr2=0.5;
for t=1:q
   c(t)=1/(1+exp(-Y(t))); %输出层输出
end 
```