今天突然遇到myeclipse闪退，搜了一下解决办法，靠谱的有下面两种（亲测有效）
两种解决办法：
<font color='red'> 1.删除问题所在工作空间下的.metadata/.plugins/org.eclipse.e4.workbench/workbench.xmi（推荐）</font>
<font color='red'> 2.删除问题所在工作空间下的.metadata，这样的话对应的myeclipse配置，诸如字体大小，编码方式等配置也会被删除，你需要进行重新配置一遍，比较麻烦。
</font>