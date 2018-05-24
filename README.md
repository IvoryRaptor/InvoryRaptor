# InvoryRaptor
项目介绍

<br/>InvoryRaptor平台为全球首个开源的物联网神经网络平台（IOTNN），打造一个使物联网设备集群按照神经网络模式进行运行的平台，
通过模拟人脑的神经网络以期能够实现类人操控处理的能力。

&nbsp;&nbsp;首先先分析一下人体对于感知外部事物并响应的过程，人体具备大量的传感器设备（眼、耳、鼻、舌、身），大量的"传感器"将大量的数据传输到大脑中进行处理，
这个传感器网络也就是人体的物联网，通过人脑进行处理，并得出结论及指导行动。人脑是由神经元组成的神经网络，神经网络是一个非常复杂的组织（成人的大脑中估计有1000亿个神经元之多）。
<div align=center>
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/dn.jpg" alt="system" title="system" width="265" height="187" />
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/sjy.jpg" alt="system" title="system" width="398" height="237" />
</div>
&nbsp;&nbsp;一个神经元通常具有多个树突，主要用来接受传入信息；而轴突只有一条，轴突尾端有许多轴突末梢可以给其他多个神经元传递信息。
轴突末梢跟其他神经元的树突产生连接，从而传递信号。这个连接的位置在生物学上叫做“突触”。
人脑对于事务的处理往往交给不同的神经元
从物联网角度来说，每个物品会发送不同类型的消息，而这些消息会被不同的"神经元"进行处理，处理的结果会传导到另一些"神经元中"
<div align=center>
<img src="https://github.com/IvoryRaptor/InvoryRaptor/blob/master/resource/nn.png" alt="system" title="system" width="367" height="278" />
</div>
&nbsp;&nbsp;在本平台中的神经元叫做Angler，每个Angler负责处理某一类传导过来的信息，这些信息可能来自于输入层，也可能来自于其他Angler处理后的消息。
物联网设备接入到平台后，由POSTOFFICE将信息根据设备类型及信息类型进行分类，传递给不同的"神经元"组中进行处理，
处理的结果传递给其他相关的"神经元"，