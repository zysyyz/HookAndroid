# HookAndroid

Hook Android系统的各个组件；该工程用于展示hook技巧。


hook Activity的启动
1. ActivityService是进程的主线程，它使用单例模式可以获取唯一对象。
2. Activity的生命周期调度，通过ActivityService中的mInstrumentation间接调度。
3. 实现Instrumentation的代理对象类，它继承自Instrumentation类，并保存原对象为oldInstrumentation，并实现接口execStartActivity。
4. hook首先获取ActivityService的单例对象，然后生成代理类InstrumentationProxy的对象，设置到ActivityService的mInstrumentation对象中。
5. 注意hook后逻辑在execStartActivity中（其中调用了oldInstrumentation对象）；android P已经对隐藏API做了限制，已经不可用。

动态代理
1. Proxy生成的是类字节码流，和普通class的唯一区别就是，它存在于虚拟机内存中而不是文件。
2. Proxy只能对接口类生成代理对象，还需要一个实现InvocationHandler接口的处理方法，该方法代理原有对象并在invoke中进行截获处理
