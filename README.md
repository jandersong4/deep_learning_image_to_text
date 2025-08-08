# Image-to-Text com CNN e GPT-2

Este projeto foi desenvolvido como trabalho final da disciplina de **Aprendizado Profundo** da UFMG e tem como objetivo implementar um sistema de **Image-to-Text** — geração automática de legendas para imagens — integrando visão computacional e processamento de linguagem natural.

## 📌 Objetivo

Gerar descrições textuais coerentes e contextualmente relevantes a partir de imagens, explorando a conexão entre visão e linguagem, com aplicações em acessibilidade, indexação de imagens e sistemas inteligentes.

## 🔍 Relevância

- **Conexão entre visão e linguagem**: integra modelos visuais e de linguagem natural.
- **Acessibilidade**: auxilia usuários com deficiência visual.
- **Busca e organização**: melhora indexação e recuperação de imagens.
- **Base para aplicações avançadas**: útil para assistentes virtuais e robótica.

## 🧠 Abordagem

O modelo é composto por:

- **Encoder**: CNN customizada, implementada do zero, para extrair embeddings visuais.
- **Decoder**: **GPT-2** pré-treinado, adaptado para receber embeddings de imagens como prefixo de entrada.
- **Treinamento**: fine-tuning seletivo no GPT-2, mantendo congeladas as camadas iniciais.

## 📊 Dataset

- **Base**: [DeepFashion](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html)
- **Subconjunto**: 3.000 imagens de peças de roupa, cada uma com descrição curta.
- **Pré-processamento**:
  - Redimensionamento para 224x224 pixels
  - Normalização com médias e desvios do ImageNet
  - Tokenização com `GPT2Tokenizer`
- **Data Augmentation**:
  - Flip horizontal
  - Crop aleatório e resize
  - Jitter de brilho e contraste

## ⚙️ Arquitetura

### Encoder (CNN)

- 4 blocos convolucionais com:
  - Conv2d → BatchNorm → ReLU → MaxPool
- AdaptiveAvgPool e camada Linear para projeção final.

### Decoder (GPT-2)

- Projeção do embedding visual para o espaço de embeddings do GPT-2.
- Concatenação com embeddings de tokens textuais.
- Máscara de atenção adaptada.
- Fine-tuning nas últimas 4 camadas e na `lm_head`.

### Modelo Final

```text
Imagem → Encoder (CNN) → Embedding visual → Decoder (GPT-2) → Legenda
```
