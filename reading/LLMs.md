# LLMs

## LLMs Distillation

[Reference](https://www.datacamp.com/blog/distillation-llm)

The bases of this approach is to have a teacher model, that was trained with a huge amount of parameters and computational resources, and a student model. The student, tries to mimic the teached knowledge, but with a smaller amount of parameters and computational resources. It approach is called distillation. It can be used amount smaller computer architectures, like mobile phones, microcontrollers, etc. And also outperforms smilar performance of the teacher model.

## DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning

[Reference](https://media.licdn.com/dms/document/media/v2/D4D1FAQFlqvgAUeuRCQ/feedshare-document-pdf-analyzed/B4DZSe94m9HIAY-/0/1737833822017?e=1738800000&v=beta&t=GlbHHcCBiNANn8W1kQrK5NWxlE9ALjO3VyIhfQBOMBI)

The article "DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning" presents two reasoning models, DeepSeek-R1-Zero and DeepSeek-R1, developed to enhance reasoning capabilities in large language models (LLMs) through reinforcement learning (RL).

Summary of Key Points:

* DeepSeek-R1-Zero is a model trained solely through RL without any supervised fine-tuning (SFT). It demonstrates significant reasoning abilities but struggles with readability and language mixing.
To improve upon these shortcomings, DeepSeek-R1 was introduced, which utilizes a multi-stage training approach and incorporates cold-start data. That is a technique that allows the model uses "formatted" data to improve the readability and language mixing.

* The authors also open-source both models and six distilled versions of varying sizes (from 1.5B to 70B parameters) based on architectures like Qwen and Llama, aiming to support further research and development in reasoning models.
