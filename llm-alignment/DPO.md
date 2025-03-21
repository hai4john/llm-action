


给 NLP 引入了一些 RL 相关的复杂性: 既要构建一个好的奖励函数，并训练一个模型用以估计每个状态的价值 (value) ; 又要注意最终生成的 LLM 不能与原始模型相差太远，如果太远的话会使得模型容易产生乱码而非有意义的文本。该过程非常复杂，涉及到许多复杂的组件，而这些组件本身在训练过程中又是动态变化的，因此把它们料理好并不容易。



 Direct Preference Optimization，论文提出将现有方法使用的基于强化学习的目标转换为可以通过简单的二元交叉熵损失直接优化的目标，这一做法大大简化了 LLM 的提纯过程。




DPO 与 PPO

在通过 RL 优化人类衍生偏好时，一直以来的传统做法是使用一个辅助奖励模型来微调目标模型，以通过 RL 机制最大化目标模型所能获得的奖励。
直观上，我们使用奖励模型向待优化模型提供反馈，以促使它多生成高奖励输出，少生成低奖励输出。
同时，我们使用冻结的参考模型来确保输出偏差不会太大，且继续保持输出的多样性。
这通常需要在目标函数设计时，除了奖励最大化目标外再添加一个相对于参考模型的 KL 惩罚项，这样做有助于防止模型学习作弊或钻营奖励模型。


DPO 绕过了建模奖励函数这一步，这源于一个关键洞见: 
从奖励函数到最优 RL 策略的解析映射。这个映射直观地度量了给定奖励函数与给定偏好数据的匹配程度。
有了它，作者就可与将基于奖励和参考模型的 RL 损失直接转换为仅基于参考模型的损失，从而直接在偏好数据上优化语言模型！
因此，DPO 从寻找最小化 RLHF 损失的最佳方案开始，通过改变参量的方式推导出一个仅需参考模型的损失！



