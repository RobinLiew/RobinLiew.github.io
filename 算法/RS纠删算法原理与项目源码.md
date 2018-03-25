##RS纠删算法原理

- A.1  纠删码是存储领域常用的数据冗余技术， 相比多副本复制而言， 纠删码能够以更小的数据冗余度获得更高数据可靠性。 Reed Solomon Coding是存储领域常用的一种纠删码，它的基本原理如下：  给定n个数据块d1, d2,..., dn，n和一个正整数m， RS根据n个数据块生成m个校验块， c1, c2,..., cm。  对于任意的n和m，  从n个原始数据块和m 个校验块中任取n块就能解码出原始数据， 即RS最多容忍m个数据块或者校验块同时丢失（纠删码只能容忍数据丢失，无法容忍数据篡改，纠删码正是得名与此）。 
- A.2  编码原理
RS编码以word为编码和解码单位，  大的数据块拆分到字长为w的word（字长w取值一般为8或者16位），然后对word进行编解码。 所以数据块的编码原理与word编码原理没什么差别， 为论述方便， 后文中变量Di, Ci将代表一个word。
首先， 把输入数据视为向量D=(D1，D2，..., Dn）, 编码后数据视为向量（D1, D2,..., Dn, C1, C2,.., Cm)，RS编码可视为如图A.1所示矩阵运算。 下图最左边是编码矩阵， 矩阵上部是单位阵（n行n列），下边是vandermonde矩阵B(m行n列), vandermode矩阵如图A.2所示， 第i行，第j列的原数值为j^(i-1)。之所以采用vandermonde矩阵的原因是， RS数据恢复算法要求编码矩阵任意n*n子矩阵可逆。

	![这里写图片描述](https://img-blog.csdn.net/20180325135125137?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWJpbjE5OTFsaXViaW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
	
	图A.1： 编码运算
	
 	 ![这里写图片描述](https://img-blog.csdn.net/20180325135138980?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWJpbjE5OTFsaXViaW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

	图A.2：vandermode矩阵
	
- 数据恢复原理
RS最多能容忍m个删除错误。 数据恢复原理的过程如下：
（1）从编码矩阵中删去丢失数据块和丢失编码块对应行。  假设D1、C2丢失，     根据图A.1所示RS编码运算等式，我们得到如下B'以及等式。  
  	
![这里写图片描述](https://img-blog.csdn.net/20180325135147442?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWJpbjE5OTFsaXViaW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

（2）由于B‘是可逆的， 两边乘上B’逆矩阵。 
	
![这里写图片描述](https://img-blog.csdn.net/20180325135155380?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWJpbjE5OTFsaXViaW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

（3）得到如下原始数据D的计算公式 
	
![这里写图片描述](https://img-blog.csdn.net/20180325135202691?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdWJpbjE5OTFsaXViaW4=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

（4）对D重新编码，得到丢失的校验码

矩阵求逆采用高斯消元法，   需要进行实数加减乘除四则运算，无法作用于字长为w的二进制数据。 为了解决这个问题， RS采用伽罗华群GF（2^w)中定义的四则运算法则。 GF(2^w）域有2^w个值， 每个值都对应一个低于w次的多项式， 这样域上的四则运算就转换为多项式空间的运算。  GF(2^w)域中的加法就是XOR， 乘法比较特殊，需要维护两个大小为2^w -1的表格: log表gflog，反log表gfilog。 
乘法公式：  a * b = gfilog(gflog(a) + fglog(b)) % (2^w -1)

##Github项目源码
源码来自：https://github.com/Backblaze/JavaReedSolomon
如果你想处理使用RS纠删算法处理字节数组，可以参照我的GitHub项目（在上一链接的基础上我进行了接口和实现的编写）
- GitHub链接：  https://github.com/RobinLiew/JavaReedSolomon
- 使用例子

```
	package com.backblaze.erasure.robinliew.dealbytesinterface;
	
	/**
	 * 
	 * @author RobinLiew 2017.9.21
	 *
	 */
	public class test {
		public static void main(String[] args) {
	
		IRSErasureCorrection rsProcessor=new RSErasureCorrectionImpl();
	
		byte[] data=new byte[1000]; 
		for(int i=0; i<data.length; i++) {  
		    data[i] = 1;  
		}  
		for(int i=0; i<500; i++) {  
		    data[i] = (byte) (16 + i);  
		}  
	
	
		int sliceCount=4;//The data is 4 copies(数据为4份)
		int fecSliceCount=2;//2 copies of erasure redundancy(纠删冗余为2份)
		int sliceLength=data.length/sliceCount;
		byte[] en_data;
		en_data=rsProcessor.rs_Encoder(data, sliceLength, sliceCount, fecSliceCount);
	
	//==================Test use: second pieces of data are lost, and the decoding code has the corresponding test code(测试使用：让第二片数据丢失，解码代码中也有对应的测试代码)=====
		byte[] temp = new byte[250];
		System.arraycopy(temp, 0, en_data, 250, 250);						
	//==========================================================================================================
	
		boolean[] eraserFlag=new boolean[sliceCount+fecSliceCount];
		for(int i=0;i<eraserFlag.length;i++){
			eraserFlag[i]=true;
		}
		eraserFlag[1]=false;
	
		int result=rsProcessor.rs_Decoder(en_data, sliceLength, sliceCount, fecSliceCount=2, eraserFlag);
		System.out.println("complete test!");//测试完毕！
		}
	
	}
```
	


