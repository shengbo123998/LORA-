# LORA-

1. MultiLoRA: Multi-Directional Low-Rank Adaptation for Multi-Domain Recommendation
   传统方法的缺点：
   1.然而，大多数现有的MDR方法基于多任务学习(MTL)范式，其中训练一个统一的模型来服务于所有领域。[35]这些方法通常为特定业务场景量身定制，导致兼容性差和部署能力有限。[29]例如，STAR 中的基于完全连接网络的星型拓扑[27]这可能会阻碍它与其他模块(如因子化机器)的集成。此外，基于MTL的模型会从多个领域接收监督信号，这不可避免地会导致冲突和负向迁移。[371.更糟糕的是，它们中的大多数严重依赖于重叠的用户或项目。[35]，这在实际应用程序中通常不可用。
   2，一方面，预训练和微调范式在。 具体来说，预训练了一个通用模型使用来自所有领域的数据，然后在目标领域进行微调这种模式几乎可以与任何模块或结构兼容，并且模型只接收来自目标域的信号，而微调、缓解冲突和重叠依赖。如何然而，完全微调为每个领域构建整个领域特定的模型导致高昂的计算和存储成本。在小任务上进行微调也可能导致灾难性的遗忘，使模型忘记在预训练期间学到的共同知识。  然而，这些方法忽略了推荐任务和自然语言处理任务之间的根本区别。推荐模型严重依赖于基于ID的嵌入，这严重阻碍了通用大规模推荐模型的发展。

   方法：
   1. 手动选择辅助领域划分。
   2. 预训练:我们使用数据集中的所有样本对骨干网络进行预训练，捕捉跨域共享的常见偏好模式。
      多方向低秩适应:我们冻结嵌入层和输出层，并微调特征交互层。具体来说，我们采用参数高效的微调(PEFT)技术，将预训练模型低秩适应到目标域。由于自然划分的域可能包含非同分布的样本，我们引入额外的辅助域划分，并训练另一组LORAS。在4.5节中进一步讨论了辅助域划分的选择。
      LORA Fusion:在图像生成领域观察到LORA的可堆叠性启发下，我们的目标是同时利用两套LoRAS进行联合预测。为此，我们训练了一个LoRA融合模块。

公式：
![image](https://github.com/user-attachments/assets/d8624d6a-a55a-453e-bdc3-bde605f8124d)
参数初始化：
![image](https://github.com/user-attachments/assets/5fc4d463-9642-410f-8f81-ff0bed5fbf3e)
PN层，但是会训练y跟b
![image](https://github.com/user-attachments/assets/92d1459f-d88b-4164-96e0-6c65814b816d)

      由于自然域划分可能会将非同分布的样本分配给相同的域，而简单的更细粒度的域划分会加剧数据稀疏性，因此我们手动选择一个标准，并在样本空间上进行辅助域划分。在自然域和辅助域划分下，我们训练了两组指向不同方向的LORAS。

      融合： 受此启发，我们的目标是结合在多个领域划分下获得的特定领域的LORAS，共同促进更准确的推荐。对于图像生成，组合权重通常由人工确定。[231.我们设计了一个LORA融合模块来计算权重。我们假设一个样本r在自然域划分下属于域A，在辅助域划分下属于域B。让对应于域A和域B的LORA模块分别为LoRAa和LORAB，包含从两个不同角度的域特定偏好模式。我们设计了一种新颖的注意力模块，用于动态生成权重并融合这些LoRA模块。 这是更细粒度的领域划分。包含从两个不同角度的域特定偏好模式。
      ![image](https://github.com/user-attachments/assets/77445dc8-64bd-4b87-8e42-a7dd068b1466)
      ![image](https://github.com/user-attachments/assets/b807d8f2-4c02-4493-b7b7-df0034cd85cc)

      

